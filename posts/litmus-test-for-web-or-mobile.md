---
permalink: litmus-test-for-web-first-or-mobile-first
title: A litmus test for Web-First or Mobile-First
languages:
published: true
summary: Think about your users and when they need your product the most.
---

I have a simple theory for whether companies should pursue web or mobile first.

Fred Wilson and Mark Suster have written impactful articles on the subject of mobile first (and web second) [here](http://avc.com/2010/09/mobile-first-web-second/) and [here](http://www.bothsidesofthetable.com/2012/01/28/web-second-mobile-first/) respectively. Fred mentions the power of mobile for engagement and Mark describes the pros of an integrated experience across all platforms.


I acknowledge that they both have infinitely more experience and success than I do. However, I think Mobile First, Web Second is too generalized.

Here is my litmus test:

> Are your users using your product for fun or work? 
> Fun should be built for mobile, work (or focus) requires the desktop/web

# B2B or B2C?

If your business is a content/consumption business, as many in the tech space are in one way or another, starting with web could be a mistake. Your users are need content to fill their boredom, and want to read quick bits on their mobile devices while they are on the go. This includes many businesses that are B2C, because consumers use their phones and devices more than their computers when they are relaxing. Locaiton-based products are also best for mobile, obviously, and are generally B2C as well.

Selling B2B can more readily be utilized on the desktop, where your users (which are always people after all) are using your product at work on their work computers.

*Of course, some products are suitable for both, and this test does not fit all businesses, but I will provide some examples.*

## Mobile First (B2C)
Optimize for mobile if your app uses:
- content consumption
- location
- camera

Content consumption should be a mobile-first business because (1) *your users are bored and using your app for entertainment* and (2) *your users need only engage for a few seconds/minutes*.

Location and camera based apps are self explanatory.

Some examples: Foursquare, Pinterest, Facebook, Instagram, Twitter (consumption unless publishing), Quora (consumption unless writing), Lyft, Uber, GrubHub, Mailbox (yes, email is a huge source for consumption)

## Web First (B2B)

Optimize for web if your app is for:
- work
- long research-ish tasks

Products assisting with work and long tasks should be web-first because (1) *your users need a keyboard for most kinds of work*, (2) *your customers need time and focus, which is easier at a computer*, (3) *they may need a file management system*, and (4) *the other software and tools they are using are probably on their desktop*.

Some Examples: real estate (42Floors, Lovely, etc), analytics companies (Mixpanel, New Relic, Heap, etc), sales (Close.io, PipeDrive, etc), project management (Basecamp, Blossom.io, etc), collaboration (Hipchat, FlowDock, Google Drive), writing or website tools.



# Some Interesting Examples


### LinkedIn

LinkedIn is a marketplace business - requiring millions of users to have profiles and then selling access to that information to two types of people: job seekers and recruiters. The average (mode) user on LinkedIn is a bystander waiting for private messages and notifications of people viewing their profiles. There is a content distribution and consumption aspect that I won't go into for now.

These bystanders like mobile because it is a quick way to consume content, see push notifications and impulsively check for updates -- none of which take a significant investment in time or effort. Mark Suster mentions that he likes LinkedIn mobile, and my guess is that this is for consumption and he does not actively network on his phone.

Recruiters and jobseekers use LinkedIn in large batches, the former using it all day at work. These use cases are ideal for a web implementation. LinkedIn would be wasting effort building a full recruiting toolkit into their mobile product. There is no need because recruiters use their product for work, at work, on their computers.


### Close.io
[Close.io](http://close.io) is a fantastic software product created by Steli Efti of [Elastic, inc.](https://elasticsales.com/) Close.io has a strong desktop application (I'm going to group this into 'web') because it's primary users are salespeople using the sales software at work. A mobile product is a great supplement for entrepreneurs and salespeople on the go.

Other sales and project management tools follow this same pattern.

### Twitter

Twitter is a networking and publishing product. People who are using Twitter for social networking or content are their strongly mobile users. People tweet about their current thoughts or what they are doing, and read on Twitter in their free time (or are bored).

Twitter on the web and desktop are useful for people who use it at work. People who rely on it for networking, publishing, or keeping up on industry news. These are the web power users. For this reason, Twitter could easily optimize different tools and features for web and mobile users. While there is some cross-use, I believe this distinction is true.

Any writing/reading platform has this split as well: readers will read on mobile, writers will write on web.

### Coderbase.io

At [Coderbase.io](http://coderbase.io), people write and recruit using their desktop computers, so we enable writing, and show profile information prominently on the desktop. Article consumption, browsing, and ideation are mobile activities. On mobile, post content is prominent and the profile information is secondary.

Full writing tools are not included for mobile (our publishing is managed through Markdown and Git, you really should try it), but we are in the process of building tools for your mobile device to help turn ideas into posts.


## Conclusion

You need to put yourselves in shoes of your users and imaine what activities they are doing and what devices they will be doing it on. 

