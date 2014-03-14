---
permalink: convert-entire-ruby-object-to-save-in-database
title: Convert an entire object to a string and save to database column in rails
languages: ruby, rails
published: true
summary: Saving whole objects into the database is time consuming but can be very useful.
---

Saving whole objects into the database is time consuming but can be very useful. When you need to store an entire object and preserve it just the way it is, using Marshal is the best bet.

> Note: PostgreSQL allows you to use the json data type. This is more efficient and a better choice if you need to hold hash-like values, but will likely not work as well when the object needs to be preserved exactly as-is

While developing Coderbase.io, I needed to store a whole object into one of our models. (this has since changed, howver)

First, I created a column with type 'text' in my model.

    class CreateModel < ActiveRecord::Migration
      def change
        create_table :connections do |t|
          //...
          t.text :blob

          t.timestamps
        end
      end
    end

I added a text column called ‘blob’ to store the object in question.

Then I overrode the getter and setter methods for the blob attrijbute in the Connection class

    class Model < ActiveRecord::Base

      belongs_to :user

      def blob=(object)
        self[:blob] = ::Base64.encode64(Marshal.dump(object))
      end

      def blob
        Marshal.load(::Base64.decode64(self[:blob]))
      end
    end

`self[:blob]` access the attribute in it’s raw form without calling the method normally used to access it when you call connection.blob. We wouldn’t want to call the method inside it’s own method.

[Marshal.dump](http://ruby-doc.org/core-2.0.0/Marshal.html) takes the object and converts it into a byte stream, and then Base64.encode64 encodes this byte stream into a string of characters that can easily be stored in a text column. calling ‘blob’ undoes the process and you end up with an actual object.