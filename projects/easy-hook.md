---
title: Easy Hook
permalink: easy-hook
description: Easily add before and after callbacks to ruby methods with blocks.
homepage: 
languages: [ruby]
visible: true
order: 
# Github Flavored Markdown reference
# https://help.github.com/articles/github-flavored-markdown
---


= EasyHook
<em>Easily add before and after callbacks to ruby methods with blocks.</em>

== Introduction

Simple hooks to add to a list of methods in your classes using blocks. All that is needed is a simple +hook_before+ and +hook_after+ which are passed a +:symbol+ and block. You are able to access the instance it is called on, as well as the method and argument information, and the returned value of the original method for the +hook_after+.

The original method's return value is still returned at the end of the +hook_after+ callback.

== Example

Much easier to show:
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
== Details
+this+ references the instance that the method was called on.

+details+ contains the method, arguments, and for +hook_after+, the return value
    details.inspect
    # => {:method => :say, :args => ["How are you?"], :return => true}

If we changed +hook_after+ to:

    hook_after :say do |this, details|
        puts "#{this.class.inspect} named '#{this.name}' called method '#{details[:method]}' with argument '#{details[:args]}', returned '#{details[:return]}'"
    end

We would get:

    p.say('How are you today?')

    # Hi!
    # How are you today?
    # Person named 'John' called method 'say' with arguments '["How are you?"]', returned 'true'


== Multiple callbacks
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
    #we replace hook_after with:
    hooks_after :say, :yell, :whisper do |this,details|
        if details[:method].to_s == 'yell'
            puts "#{this.name} yelled '#{details[:args].first.upcase}'"
        else
            puts "#{this.name} said '#{details[:args].first}'"
        end 
    end


== Timer

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


And here is it's use with +hook_before+ and +hook_after+.

    include EasyHook
    hook_before :sum do |this, details|
        @sumtime = HookTimer.start
    end
    #we replace hook_after with:
    hook_after :sum do |this,details|
       puts "#{HookTimer.stop(@sumtime)} seconds"
    end
    



== Acknowledgements

Much credit to Cristoph Heindl (http://cheind.blogspot.com/).