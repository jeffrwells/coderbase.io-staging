---
permalink: enable-subfolders-for-rails-models
title: Enable subfolders for Rails models
languages: ruby, rails
published: false
summary: Use subfolders in your Rails models directory for better organization
---


## subfolders

I’m using Single Table Inheritance (STI) in coderbase to represent different kinds of events. I have a base ActivityEvent class that I want in the models folder, with several subclasses of events. Putting these in an activity_events folder won’t load unless you add this to your application.rb file:

    config.autoload_paths += Dir["#{Rails.root}/app/models/**"]

The double star matches files and folders recursively, so everything inside that folder will be loaded
[http://ruby-doc.org/core-1.9.3/Dir.html#method-c-glob](http://ruby-doc.org/core-1.9.3/Dir.html#method-c-glob)
