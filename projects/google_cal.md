---
title: Hero Scheduler
permalink: hero-scheduler
description: A tool for scheduling rotating shifts on Google Calendar for teams
homepage:
languages: [javascript]
visible: true
order: 5
---

This is a simple calendar application for creating 'Hero' schedules. That is, every week one member of a team takes a hero duty for the week, such as working support or fixing bugs. This application is a Sinatra app that is run locally, and posts information to Google Calendar.

It is meant as an internal tool and is not very refined. Running `rake calendar` runs the tool's server and opens [http://localhost:9292](http://localhost:9292) to use the application.

The application uses DataMapper and SQLite to persist information about the team members. This information is just saved using Git as the rake task automatically pushes it up.

The hardest part of this simple application was using Google authentication in Sinatra and the Google Calendar API. The Google Calendar API has a potential bug in it, in that it returns results in pages, but does not condense deleted records. For example if I wanted the first 100 records, but I had deleted 50 records, then getting the first page of 100 would only return 50 results. Since my application removed events and created them again every time it updated (this was the only plausible way to ensure cycles of events were correct), it deleted a lot of events. To overcome this error, I had to set the page limit extremly high.
