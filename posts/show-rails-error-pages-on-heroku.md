---
permalink: show-rails-error-pages-on-heroku
title: Show rails error pages on heroku
languages: ruby, rails
published: false
summary: Pre-launch it can be useful to show error pages on Heroku so you can debug production issues.
---

One thing I dislike about heroku is that it feels like a black box to me. Once my code is on their servers, I can’t touch it or see what it is doing very well. If possible, I prefer to be able to know what is going on. For finding errors when just playing around with heroku (not production level code), I like to show the Rails error pages. Here’s how:

    // config/environments/staging.rb

    config.consider_all_requests_local  = true



If you aren't using it yet, you may want to use [Better Errors](https://github.com/charliesome/better_errors), it has nice formatting, and a REPL to see what is going on when an error occurs.

Don’t be surprised if this doesn’t work on heroku though, As described in the [README](https://github.com/charliesome/better_errors), because of the REPL, it only works on whitelisted IPs (namely localhost).

Then you can add this to your staging.rb file

    BetterErrors::Middleware.allow_ip! ‘IP’

There readme suggests using `ENV['TRUSTED_IP'] if ENV['TRUSTED_IP']`
and `TRUSTED_IP=66.68.96.220 rails s`, but this will be a hassle in heroku.
Showing error pages and adding the REPL in any public, unauthorized location is a VERY BAD IDEA(™). This is really only suggested for debugging why features working locally won’t work on heroku, when testing in a test or staging environment
