---
title: Coderbase
permalink: coderbase
description: A portfolio & blogging platform for developers powered by git and markdown
homepage: http://coderbase.io
languages: [ruby]
visible: true
order: 1
# Github Flavored Markdown reference
# https://help.github.com/articles/github-flavored-markdown
---


# Git Branching Strategy
* Feature branches: create a new branch for every little feature: f-feature-name

# Database
1. Install Postgres using Heroku's app: http://postgresapp.com/
2. Run `PATH="/Applications/Postgres93.app/Contents/MacOS/bin:$PATH"` in terminal

# Coding Standards
## Ruby
- One class per file
- 2 spaces per tab and spaces as tabs (Sublime setting)
- For comments in HAML views, use Ruby so the comments don't show in front-end (unless they need to)
- Short, concise methods favoring more methods over method verbosity
- Most logic should not go in models; models should be skinny. `/lib` should contain classes for logic stuff
## Javascript
- Adam?

# Dev Server
 - You must run on localhost and not 0.0.0.0 because only localhost will work with Github's callback
 - run `forward 3000` for GitHub callbacks

# Decisions to make
* We should make a team/about page giving it a personal feel. maybe part of the homepage?
* When to launch on HN? Balance best time of day/week with our availability.
* Add tooltips or something around our madlibs to show how to modify (while logged in)
* What's our hash tag? #coderbase ?
* How do we all see each other's responses to emails to users? BCC: team@ 
* team@ might not be forwarding correctly?

# HN Launch plans
* Launch on Product Hub first
* Turn on extra dyno to prevent app spinup delay
* Upgrade database to eliminate 10k row limit
* Coordinated email to friends/colleaguse asking for upvotes (with specific instructions to prevent voting ring)
* Coordinated tweets/emails to semi-famous developers asking to try us out
* Commenting strategy to questions on HN

# Landing Page Ideas
* Team photos with links to each of our coderbase profiles
* Why we did this? Tell the story
* Login button
* What happens when a user goes to the homepage while logged in?
* If we show laptop on landing page, we should show iPhone on responsive iPhone design (or detect user-agent)


# Feedback
- Custom styling in YAML or or css file
- custom domain
- searchability
- following other people
- is this searchable?
- robots.txt
noah:
- multiple tweets
- tweet if #cbio hashtag
- hashtag tweet creates a postuse the commit message.
- list of posts vs list of 
- easter egg: curation
- #hashtag in a commit shows the diff, and commit message
acccraze:
- confused between projects and posts
- overall, positive, thoguht it was very clever
- currently runs on wordpress, already had it installed so just kep tiusing it
- hostgator
- wants customd domain
- would switch over right away
- only blogs once a semester, because it takes too much time. 
- would send out current website accraze.info
- hate designing, something read to go and intuitive, he's sold on it
Wes
- doesn't make sense to have prepopulated empty projects
- what does the right sidebar do
- Why isn't my fork visible
- How to I change visibility
- needs tour to explain process

to be main site
- here's who I am
- here's my work experience
- introduction
- doesn't feel like an identity yet
- madlibs not enough to describe myself
- separate projects and blog from identity

- ABOUTME
- write an opening statement, opportunities I'm looking for.
- Languages
  - we pulled this from 

- no landing page, just shows a post.
- time.

BIGGEST THING
  separation of who I am and then my blog
NICE TO HAVE
  How do I separate myself?
  Design?


- would use the service because how easy to

- personalization

- wants a platform easier than what he has
  ghost was hard to set up
  he writes posts in markdown, then converts to html, then publishes on wordpress



TODO
================================================================================

All
- [ ] Compile list of semi-famous devs we want to tweet/email asking to try us out
- [ ] Compile list of friends to email for HN voting
- [ ] Parameterize project titles to make them URL friendly



Bugs
----------------------
- [ ] routes don't work if period in permalink
- [ ] What to do if no name from github?


Tasks from 2/16
--------------------------------------------------------------------------------
- [x] Put a char limit on description in project box in middle column
  - Is there any reasonable way to do this? Maybe just add comment in config file
  - No need for this now, project card can just expand

- [ ] Recommendation algorithm for next post based on language, project and recency

- [ ] Hosting site.

Jeff
--------------------------------------------------------------------------------
- [x] Twitter Setup
- [x] When should we fetch most recent tweet. (asynchronously)
- [x] Hipchat
- [x] Post list view
- [x] Redo templating
- [x] Modal functionality
- [x] Clean up db records
- [x] Project post needs separate url, no redirect?
- [v2] Conflict warning (!)
- [x] Responsive basics, mobile
- [x] Seed only one sample post.
- [x] Convert . to - in project permalink, like davis.js
- [x] conflicts model
- [x] What to do with posts under projects
    - [x] regardless, don't break, that's not the functionality we want
    - [x] secondly, don't have the seed post be under a hidden project
- [x] Add twitter back to config
- [x] Change languages to read array, not comma separated string
- [x] Fix twitter handle in config to work with and w/o @ symbol
- [x] Make mad libs start blank, except for the auto-filled in company
- [x] Remove gists from access on auth page
- [x] Move GitHub app to new coderbase organization on GitHub
- [x] On progress page after sign up authentication, update text (use terms like profile, not repo)
- [x] Add edit button on hover to madlibs to show it is clickable.
- [x] Remove "projects" and "posts" icon/bar in the middle column. Project cards should go to top
- [x] Add new link for a new post file
- [x] Change routes to new structure: 
    /username, 
    /username/project-name
    /username/root-post-title
    /username/project-name/post-title-with-project 
    /username/post-title-with-project => (303) /username/project/post-title-with-project
- [x] Help icon should bring up get started modal
- [x] In project list, show project title (if exists) or repo title (if project title doesn't exist)
- [x] Stack top right actions vertically, 2 buttons: list view (replaces /writing) & menu. Menu has login/logout, help, chat/email, and help/tour modal. 
- [x] Add 'draft:true' still visible if you are logged in?

- [x] error output to post.

- [x] nil values for twitter, etc.

- [x] Clean up route constraints?
- [x] remove conflict modal
- [x] Logout
- [x] Fix the YAML errors/markdown preview/notification problem
  - [x] Make sure post still updates if YAML fails. Figure out yaml failures. 
  - [x] Add markdown documentation. Code fences problem
  - [x] Look into YAML options
  - [x] Add syntax errors to the post with comments? (won't update local)
  - [x] Add syntax errors to user config?? maybe email. - we can just email them, will show up on airbrake.
- [ ] spaces in folder names should be permalinked.
- [x] Style Olark
- [ ] Test multiple file additions and deletions in Github hook controller
- [ ] Set up customer.io with written automated welcome emails
- [x] long titles on flyout list need indent or icon or something
- [x] Flyout list does not scroll
- [ ] fix adam's dev
- [x] Ajax project loads.
- [ ] Make the social sharing buttons work
- [ ] Order


Sean
--------------------------------------------------------------------------------
- [X] Set up domain on heroku
- [X] Set up segment.io
- [X] Google Analytics
- [X] Heap
- [X] Add Olark
- [X] Setup Airbrake
- [X] Buy SSL cert
- [X] Create GitHub organization for Coderbase
- [X] Setup SSL
- [X] Enforce SSL on every request
- [X] Redirect www to root
- [X] Enforce Olark: Always on homepage or user's own profile if logged in
- [X] Reissue SSL cert to make work for both www are root
- [X] Support custom domain
- [ ] Change starter repo name away from coderbase-get-started, different names for development and production.
- [ ] Landing page
- [ ] Setup Staging environment
- [ ] Set up indicies on db / Speed
- [ ] Write email to friends asking for upvotes
- [ ] Email copy for segment.io/customer.io
New Tasks
- [ ] Write tests for custom domain stuff
- [ ] Refactor custom domain redirector into class

Adam
--------------------------------------------------------------------------------
- [ ] Responsive magic
- [X] Design for posts icon
  - need this anymore??
- [X] Reposition name & photo so it's higher in the column, more in balanced, and photo is bigger
- [X] Stack top right actions vertically, 2 buttons: list view (replaces /writing) & menu. Menu has login/logout, help, chat/email, and help/tour modal. 
- [X] Reposition name & photo so it's higher in the column, more in balanced, and photo is bigger
   - Jeff: gave you a head start on this but please center/space to your preference.
- [ ] Hover to show language text as language label
- [X] Project box design in middle column: Project title, short desc, edit link
- [X] Add project header with title, longer description, link to repo, Github indicator icon (to indicate it's a Github repo), number of Github stars, website URL, forked icon (if the repo was forked)
- [X] By default forked, private and collaborator repos are hidden, should this change?
    - Adam: I think they should be hidden.
- [ ] Update auth description on GitHub authentication page
- [X] Reposition tweet and twitter handle to have more balance
- [ ] Write & design get started modal
- [ ] Auto-load hello modal on first sign up with a few second delay to show the user what their profile looks like
- [X] No posts page, No posts menu list
- [X] Flyout list does not scroll
- [X] long titles on flyout list need indent or icon or something

Launching
================================================================================


Post-RAD testing
--------------------------------------------------------------------------------

- [ ] Twitter Campaign
- [ ] Javascript/Ruby weekly

Scaling concerns if this takes off on Hacker News
--------------------------------------------------------------------------------

* Monitor total database rows. The free Heroku database level limits total rows to 10,000. We just need to upgrade to the next $9/mo level if we max out. It's not an easy transition and requires a few minutes downtime while you switch databases. 
* average records per user = 1 user + 20 projects + 5 posts = 400 users in 10,000 rows


Launch Places
--------------------------------------------------------------------------------

1. CrunchBase http://www.crunchbase.com/company/blogvio
2. Angel List https://www.angel.co/blogvio
3. http://www.betali.st/ 39$ - http://betali.st/startups/blogvio
4. http://www.romanianstartups.com/
5. http://www.startupli.st/
6. http://www.kickoffboost.com/
7. http://www.geekopedia.me/startupsubmit/
8. http://www.killerstartups.com/
9. http://www.startupbird.com/
10. http://www.ratemystartup.com/
11. http://www.new-startups.com/
12. http://www.nextbigwhat.com/
13. http://www.leanstack.io/
14. http://www.launchingnext.com/
15. http://www.startupproject.org/
16. http://www.erlibird.com/ - they ask 199$.
17. http://www.thestartuppitch.com/
18. http://www.startuplift.com/
19. http://www.feedmyapp.com/submit/ (http://feedmyapp.com/p/a/blogvio/28928)
20. http://www.siliconallee.com/contact
21. http://www.f6s.com/
22. http://www.paggu.com/
23. http://www.aboutyourstartup.com/ (http://aboutyourstartup.com/?s=blogvio)
24. http://www.eu-startups.com/directory/
25. http://www.go2web20.net/
26. http://www.101bestwebsites.com/
27. http://www.vator.tv/
28. http://www.springwise.com/
29. http://www.techpluto.com
30. http://www.cee-startups.com/
31. http://www.appuseful.com/
32. http://www.startupwizz.com/
33. http://www.startuptunes.com/ - http://directory.startuptunes.com/b/Blogvio
34. http://www.venturebeatprofiles.com/
35. http://www.techhunger.com/
36. https://www.gust.com - https://gust.com/c/blogvio
37. http://www.cee-startups.com/
38. http://www.startupbook.co
39. http://www.launch.it/contact_form/1/0/contact
40. http://www.netted.net/contact-us/
41. http://www.minisprout.com/about/
42. http://www.makeuseof.com/contact-team/
43. http://www.venturevillage.eu/about-us/contact/
44. http://www.appvita.com/
45. http://www.webdevtwopointzero.com/
46. http://www.dzineblog.com/
47. http://www.rev2.org/
48. http://www.techattitude.com/contact
49. http://www.eastist.com/
50. http://www.siliconallee.com/
51. http://www.en.startit.rs/
52. http://www.en.startupbusiness.it/
53. http://www.blogs.wsj.com/tech-europe/
54. http://www.rudebaguette.com/
55. http://www.venturevillage.eu/
56. http://www.en.webrazzi.com/about/
57. http://www.sociableblog.com/contact-us/
* http://www.reddit.com/r/startups
* http://www.reddit.com/r/sideproject
* http://thenextweb.com/market/
* http://momb.socio-kybernetics.net/
* http://headlinr.com/
* https://www.favsync.com/common/publicTab/v274z25464-3312a1d2...
* http://www.crunchbase.com/
* https://www.angel.co/
* http://www.betali.st/
* http://www.romanianstartups.com/
* http://www.startupli.st/
* http://www.kickoffboost.com/
* http://www.geekopedia.me/startupsubmit/
* http://www.killerstartups.com/
* http://www.startupbird.com/
* http://www.ratemystartup.com/
* http://www.new-startups.com/
* http://www.nextbigwhat.com/
* http://www.leanstack.io/
* http://www.launchingnext.com/
* http://www.startupproject.org/
* http://www.erlibird.com/
* http://www.thestartuppitch.com/
* http://www.startuplift.com/
* http://www.feedmyapp.com/submit/
* http://www.siliconallee.com/contact
* http://www.f6s.com/
* http://www.paggu.com/
* http://www.aboutyourstartup.com/
* http://www.eu-startups.com/directory/
* http://www.go2web20.net/
* http://www.101bestwebsites.com/
* http://www.vator.tv/
* http://www.springwise.com/
* http://www.techpluto.com
* http://www.cee-startups.com/
* http://www.appuseful.com/
* http://www.startupwizz.com/
* http://www.startuptunes.com/
* http://www.venturebeatprofiles.com/
* http://www.techhunger.com/
* https://www.gust.com
* http://www.cee-startups.com/
* http://www.startupbook.co
* http://www.launch.it/contact_form/1/0/contact
* http://www.netted.net/contact-us/
* http://www.minisprout.com/about/
* http://www.makeuseof.com/contact-team/
* http://www.venturevillage.eu/about-us/contact/
* http://www.appvita.com/
* http://www.webdevtwopointzero.com/
* http://www.dzineblog.com/
* http://www.rev2.org/
* http://www.techattitude.com/contact
* http://www.eastist.com/
* http://www.siliconallee.com/
* http://www.en.startit.rs/
* http://www.en.startupbusiness.it/
* http://www.blogs.wsj.com/tech-europe/
* http://www.rudebaguette.com/
* http://www.venturevillage.eu/
* http://www.en.webrazzi.com/about/
* http://www.sociableblog.com/contact-us/
* http://www.producthunt.co/


Bloggers
---------------------------------------------------
- hans.io
- ridingtheclutch.com
- http://jvns.ca/
- http://blog.ezliu.com/
- http://threeifbywhiskey.github.io/
Potential v2
==========================================================================
- [ ] Script to generate posts or projects.
