---
title: easy-hook
permalink: easy-hook
description: Easily add before and after callbacks to ruby methods with blocks.
homepage:
languages: [ruby]
visible: true
order: 3
# Github Flavored Markdown reference
# https://help.github.com/articles/github-flavored-markdown
---


# Summary

*easy-hook* is a simple library for adding callbacks before and after a method call in Ruby. It also has a simple Timer module that provides a `start`,`stop`,`restart` syntax for timing events using Ruby's native Time class.

# Motivation

This library was built largely to demonstrate my understanding of timing with callbacks. I had an interview with New Relic in which I was asked a number of questions about timing events. The questions were presented rather vaguely and I had a hard time understanding what the interviewer was asking. At the end of the interview, I felt that the interviewer came away with a poor impression because of this misunderstanding. I built this gem to demonstrate that I not only understood the concept, but could implement it in a practical application.


# Implementation

## Callbacks

Here is the primary code driving the hooks.


      def hook_before(*syms, &block)
        syms.each do |sym| # For each symbol
          str_id = "__#{sym}__leading__"
          unless private_instance_methods.include?(str_id)
            alias_method str_id, sym
            private str_id
            define_method sym do |*args|
              block.call(self,{
                 :method => sym,
                 :args => args
              })
              result = __send__ str_id, *args  
              result
            end
          end
        end
      end

      alias_method :hooks_before, :hook_before

For each method passed as a symbol, the original method is aliased and a new method is defined. This one calls the block that is given and passes along the class instance, method name and method arguments. Then the original method is called (by calling the alias) and it's values are returned.

The `hook_after` method is very similar, but the original method is executed and the return value saved before the hook is called.

## Timer

The Timer is created using a Timer class that tracks various timer instances so that timers can run concurrently. `ObjectSpace` is used to keep track of timer objects using their native object ids, allowing the timers to be started or stopped at various points throughout an application. A finalizer is created to update the set of running timers.

Methods are added to a module to `start`, `restart`, `stop` to control a timer, and a `time` method is used to get the running time without stopping the timer. `Time.now` is used for timing, and the `stop` and `time` methods return the elapsed time.

The entirety of the timer code:


    module HookTimer
      require 'set'
      class Timer
        class << self
          attr_accessor :ids

          def finalize(id)
            @ids.delete(id)
          end

          def register(obj)
            @ids ||= Set.new
            @ids << obj.object_id
            ObjectSpace.define_finalizer(obj, method(:finalize))
          end


        end

        attr_accessor :time, :start

        def initialize
          @start = Time.now
          @time = nil
          Timer.register(self)
        end
      end

      def self.start
          Timer.new.object_id
      end

      def self.restart(id)
        timer = ObjectSpace._id2ref(id)
        return false unless timer
        timer.start = Time.now
        timer.time = nil
        true
      end

      def self.stop(id)
        stop = Time.now
        timer = ObjectSpace._id2ref(id)
        return false unless timer
        timer.time ||= stop - timer.start
      end

      def self.time(id)
        stop = Time.now
        timer = ObjectSpace._id2ref(id)
        return false unless timer
        stop - timer.start
      end


    end



# Usage
*Easily add before and after callbacks to ruby methods with blocks.*

## Introduction

Simple hooks to add to a list of methods in your classes using blocks. All that is needed is a simple +hook_before+ and +hook_after+ which are passed a +:symbol+ and block. You are able to access the instance it is called on, as well as the method and argument information, and the returned value of the original method for the +hook_after+.

The original method's return value is still returned at the end of the +hook_after+ callback.

## Example

    require 'easy-hook'
    class Person
        attr_accessor :name

        def say(string)
            puts string
            return true
        end

        include EasyHook
        hook_before :say do |this, details|
            puts "Hi!"
        end
        hook_after :say do |this, details|
            puts "Bye!"
        end
    end

Then these hooks are run when the method is called:

    p = Person.new
    p.name = "John"

    p.say('How are you?')

    # Hi!
    # How are you?
    # Bye!

    # => true

## Details

`this` references the instance that the method was called on.

`details` contains the method, arguments, and for `hook_after`, the return value
    details.inspect
    # => {:method => :say, :args => ["How are you?"], :return => true}

If we changed `hook_after` to:

    hook_after :say do |this, details|
        puts "#{this.class.inspect} named '#{this.name}' called method '#{details[:method]}' with argument '#{details[:args]}', returned '#{details[:return]}'"
    end

We would get:

    p.say 'How are you today?'

    # Hi!
    # How are you today?
    # Person named 'John' called method 'say' with arguments '["How are you?"]', returned 'true'


## Multiple callbacks
These callbacks become more useful when you use them to watch multiple methods.

    ...

    def yell(string)
        print string.upcase
        return true
    end

    def whisper(string)
        print string
        return true
    end

    ...

    # we replace hook_after with hooks_after
    hooks_after :say, :yell, :whisper do |this, details|
        if details[:method].to_s == 'yell'
            puts "#{this.name} yelled '#{details[:args].first.upcase}'"
        else
            puts "#{this.name} said '#{details[:args].first}'"
        end
    end


# Timer

I created a simple Timer to be used when timing the length of a method call. By starting the timer in the +hook_before+ callback and stopping it in the +hook_after+ callback you can effectively time the length of a function.

Uses Time.now, so it is very accurate.

There are four methods and various uses:

    # start a timer. Returns a unique id to allow concurrent timers    id = HookTimer.start
    #=> 438074832708

    # report the elapsed time since the timer started
    HookTimer.time(id)  
    #=> 1.34589435

    HookTimer.time(id)
    #=> 2.39487132

    # stop the timer and return the elapsed time.
    HookTimer.stop(id)
    #=> 5.38947238

    # the time method still returns time since started.
    HookTimer.time(id)
    #=> 8.38732948

    # the timer is now stopped, so the stopped time is saved
    HookTimer.stop(id)
    #=> 5.38947238

    # an invalid id will cause the methods to return false.
    HookTimer.time(123)
    #=> false

    # restart the same timer.
    HookTimer.restart(id)
    #=> true


And here is it's use with `hook_before` and `hook_after`

    include EasyHook
    hook_before :sum do |this, details|
        @sumtime = HookTimer.start
    end
    #we replace hook_after with:
    hook_after :sum do |this,details|
       puts "#{HookTimer.stop(@sumtime)} seconds"
    end
