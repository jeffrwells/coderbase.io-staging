---
title: Greekslide
permalink: greekslide
description: SaaS fraternity recruitment software
homepage: http://www.greekslide.com
languages: [ruby, rails]
visible: true
order: 4
---

Greekslide was my first ever programming project. I wanted to learn Ruby on Rails and began this project to do so. It is a management tool for fraternity and sorority recruitment. During rush week, hundreds of potential new members try to meet as many active members as possible. The way this was previously managed was with iphone photos and a paper spreadsheet.

Greekslide allows all of the active members of a fraternity to sign in with Facebook and have a private forum. Potential new members sign in (with Facebook) at events and can fill out information about themselves and answer questions. This way the new members can portray themselves how they would like. Active members can comment on these potential new members and look through a slideshow of their information. The software also includes basic list management for keeping track of the new members.

See photos of how everything worked below.

# Sales
I was able to sell this software as a subscription to multiple fraternity chapters, but have decided to shut it down. There are two reasons: (1) The product was extremely buggy and difficult to maintain. I wrote this code before I understood a lot about the internal workings of Rails or testing. It was always having errors, especially at scale, and was difficult for me to maintain. I could probably go back in and fix everything now, but it would not be worth the time. (2) It also had a terrible sales process. This software was only useful for two weeks each year -- during rush week each semester. Recurring revenue was semi-annual, and new deals could only be closed a few weeks before each semester began. Selling to fraternities was difficult as they did not have processes in place for making purchases like a typical business does, and no one knows who has the authority to make a purchase like this. In general, communication was difficult and deals would not close. I learned a lot about sales, but it was no longer profitable for me, and I shut it down.

# Photos

*You can see these descriptions and photos in the tour at [http://www.greekslide.com/tour](http://www.greekslide.com/tour).*

## All discussions in one place
![](http://greekslide.com/assets/stream_c-0c408ccadb647c1347b2e651852c5e02.jpg)

Actives discuss potential new members in a feed, where they see all of the recent conversations in one place. Actives are engaged and make an effort to discuss the new guys. Each comment has a sentiment attached to quickly understand the chapter's thoughts on a rushee. Thumbs up, thumbs down, or needs more discussion


## Quick signup for new members
![](http://greekslide.com/assets/quick_c-b4e22e8c1e0285f4855b06aed6a52099.jpg)

Actives can open up the Quick Rushee Signup page while at a rush event and have potential new members quickly sign up using Facebook. They get their name, profile photo, email address and phone number to contact them quickly and easily. Later, the new guys can log in to fill out more information about themselves.


## Potential new member profile
![](http://greekslide.com/assets/profile_c-fc55620e08e80c911412958dbdcbb053.jpg)

Potential new members fill out information about themselves for actives to see. They get the opportunity to put their best foot forward and use a great photo. That way everyone gets a fair chance to see and be seen.

## New member lists
![](http://greekslide.com/assets/rush_c-a1e5f7a3c32d2f3cb7d7d1dd4519d6ac.jpg)

Drag and drop potential new members to organize them into pref and bid lists. Each of these lists can be downloaded as excel files with all of the new members' information. There is a slideshow for each category as described below.

## Slideshow
![](http://greekslide.com/assets/slideshow_c-e798f24f1dc13ed467125bd5e288cfae.jpg)

The slideshow allows active members to step through each rushee and view all of his information as well as answers to questions. They can read the comments of other actives and learn more about potential new members they haven't yet had the chance to meet at rush events. The chapter admin uses the slidshow on vote night. They simply click the star to change his status from 'Preferred' to 'Bid'. After the vote is over, they can download the bid list to send in to their school.



# Code

As I said, this is the first code I ever wrote. It did the job but is not something I am proud to show off anymore, so I won't show any code samples.

One interesting thing I did was create my own authentication, and did not use Devise or a similar library. While my implementation was definitely less secure, it allowed me to learn how authentication, password salting and hashing, and security work. I now have a much better understanding of what libraries like Devise do.
