---
permalink: omniauth-in-development-and-production
title: Omniauth in development and production
languages: ruby, rails
published: true
summary: When setting up your rails app with omniauth, you want to use a clean setup to handle dev and production
---

When I am developing using an authentication with OAuth, I always have a hard time when moving to test my code in staging or use it on production, because your OAuth app settings require you to set your callback url, including the domain name. Often times people solve this by using `ENV[‘app_id’]` and `ENV[‘app_secret’]` which are then passed in on deploy.

For Coderbase.io, I am connecting Github, Twitter, and Google with OAuth in order to aggregate your online activities from these sources and allow you to curate them into your online presence/portfolio.

Because of this, I have 3 providers over multiple environments.

I wanted to keep these in the same place, so I use a yaml file

// config/omniauth.yml

    development:
        github:
            id: ‘development_github_app_id’
            secret: ‘development_github_app_secret’
        twitter:
            id: ‘development_twitter_app_id’
            secret: ‘development_twitter_app_secret’
        gplus:
            id: ‘development_google_plus_app_id’
            secret: ‘development_google_plus_app_secret’

    production:
        github:
            id: ‘production_github_app_id’
            secret: ‘production_github_app_id’
        twitter:
            id: ‘production_twitter_app_id’
            secret: ‘production_twitter_app_secret’
        gplus:
            id: ‘production_google_plus_app_id’
            secret: ‘production_google_plus_app_secret’

This allows me to keep all of my keys in a single YAML file. Since I keep this repo private, I don’t have to worry about my keys getting exposed.


To use it, in Rails I use:

    // initializers/omniauth.rb

    CONFIG = YAML.load_file("#{::Rails.root}/config/omniauth.yml")[::Rails.env]

    Rails.application.config.middleware.use OmniAuth::Builder do
      provider :github, CONFIG["github"]["id"], CONFIG["github"]["secret"], scope: "user:email,repo,gist"
      provider :twitter, CONFIG["twitter"]["id"], CONFIG["twitter"]["secret"], scope: "user:email,repo,gist"
      provider :google_oauth2, CONFIG["gplus"]["id"], CONFIG["gplus"]["secret"]
    end


The first line parses the yaml file above, and `[::Rails.env]` grabs the hash corresponding to the current environment when this is called.

