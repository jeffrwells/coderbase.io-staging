---
permalink: loading-heroku-postgres-db-with-rake
title: Loading Heroku Postgres db with rake
languages: ruby, rails, heroku, postgresql
published: true
summary: Load up a copy of your live database with rails
---

We are performing a design overhaul, and it is important to me to try out the new designs with some real data. 

I wanted to be able to pull down the database we have on heroku and load it into my local database. Apparently there used to be `heroku db:pull` but that is depracated. The plugins [heroku-legacy-taps](https://github.com/heroku/heroku-legacy-taps) and [heroku-taps](https://github.com/heroku/heroku-taps) did not work for me.

They have directions [here](https://devcenter.heroku.com/articles/heroku-postgresql#pg-push-and-pg-pull) and [here](https://devcenter.heroku.com/articles/pgbackups). I compiled these into a rake task to run.

> NOTE: This will delete your current local database!

    // lib/tasks/db-heroku-load.rake

    namespace :db do
      namespace :heroku do
        desc "Drop the current database and load from heroku"
        task :load => :environment do
          
          # capture a new backup and delete the oldest manual backup
          # if you don't want to delete any backups, don't use the '--expire' flag
          system "heroku pgbackups:capture --expire"
          
          # print the backups to show that there is a new one
          system "heroku pgbackups
          
          # download the latest backup and save it as 'latest.dump'
          system "curl -o latest.dump `heroku pgbackups:url`"
          
          # call pg_restore to replace your local database with the new one.
          # you will need to rename app_development
          system "pg_restore --verbose --clean --no-acl --no-owner -h localhost -d app_development latest.dump"
          system "rm latest.dump"
        end
      end
    end

> You need to replace 'app_development' with the name of your development database

You will need `pgbackups` enabled. It is free. Use this command:

    heroku addons:add pgbackups

To run this just use:

    rake db:heroku:load

If you get a problem that the `pg_restore` is not found and you are using [PostgresApp](http://postgresapp.com), then you need to add the postgres binaries to your path:

    PATH="/Applications/Postgres93.app/Contents/MacOS/bin:$PATH"
    // this may not be your path. Find the correct path with:
    sudo find / -name pg_restore


If you're curious how this works, read [the documentation](https://devcenter.heroku.com/articles/heroku-postgresql#pg-push-and-pg-pull) or ask me.

    
