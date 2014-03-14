---
permalink: keep-private-developer-info-hidden
title: How to keep private developer information hidden from the repository
languages: ruby, rails
published: true
summary: See how to allow multiple developers to deploy via capistrano while keeping developer information private
---

How to keep private developer information hidden from the repository (allow multiple developers to deploy via capistrano)

I was recently working on a project where I wanted my collaborators to be able to deploy my repository with Capistrano. Capistrano requires your `scm_username` and `scm_password`, in this case each of our github usernames and passwords. I could have put my own information into the repository, but I didn’t want my collaborators to see my password. Here is what I was able to do:


    //config/deploy.rb
    $LOAD_PATH.unshift File.join(File.dirname(__FILE__), 'deploy')
    require 'sensitive.rb'
    #ERROR: config/deploy/sensitive.rb was not found. Must set constants: GITHUB_USER, GITHUB_PASSWORD, SERVER_USER, SERVER_PASSWORD
     


    //config/deploy/sensitive.rb

    #this file should not be checked into any public repository - keep a copy on your local computer.

    SERVER_USER = "jeff"
    SERVER_PASSWORD = "my_server_password"
    GITHUB_USER = "jeffrwells"
    GITHUB_PASSWORD = "my_github_password"



    //config/deploy/sensitive.rb.template

    SERVER_USER = ""
    SERVER_PASSWORD = ""
    GITHUB_USER = ""
    GITHUB_PASSWORD = ""



    //.gitignore

    /config/deploy/sensitive.rb

deploy.rb will read in the sensitive.rb file and use the constants defined there! This file isn’t checked into the repository, so the only copy will be the local one on the developer’s computer. The sensitive.rb.template is in version control in order to make it easier for the developer to just duplicate the file, remove the .template extension, and fill in their information. If the sensitive.rb file isn’t found, because the developer did not copy the template, then it will throw an error when trying to deploy, and I left the comment in there to describe to the developer what happened.
