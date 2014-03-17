---
title: Coderbase
permalink: coderbase
description: A portfolio & blogging platform for developers powered by git and markdown
homepage: http://coderbase.io
languages: [ruby, rails]
visible: true
order: 1
---

# Summary

*Note: This project is very much a work in progress. This portfolio was created on, and is hosted by, Coderbase.io.*

Coderbase.io is a portfolio platform for developers. It seeks to create the ideal online identity for developers. It combines blog-style posts, projects that contain annotations -- such as what you are reading right now -- and basic pages.

One of the most interesting parts about it is that everything is driven through Markdown, YAML, and Git. There is no admin UI through the browser. When a user signs up through GitHub, a new repository is created on his/her GitHub account that contains all of the configurations and content.

See below for further details and pictures.


# Motivation

Hiring extremely talented software engineers is a significant challenge for tech companies today. More than any other factor, the people at a company make or break its success. For this reason, managers and recruiters go to great lengths to determine a job candidate's abilities and qualifications before making a decision. Time and effort are wasted for the employer as well as the engineer.

To attack this problem my team and I wanted to create a platform that allowed developers to show their proficiency in programming, their personality, and their ability to solve problems as well as gain exposure through writing.

We hope to replace a resume, blog, and GitHub profile with one single portfolio accessible by a custom domain name: (eg jeffrwells.com). The portfolio is well designed and minimally featured to make it easier to use and more attractive than any other service or custom-built solution.


# How it works

Coderbase.io is built for developers, and is designed to work in tune with a developer's typical workflow. We recognized early on that

When a user signs up through his/her GitHub account, a new repository called *coderbase.io* is created that hosts all of the portfolio's posts, projects, and configurations in markdown and YAML files. When the repository is modified and pushed to GitHub, a commit hook sends the changes over to us and we parse it to manage the portfolio.

![Newly created GitHub Repository](https://raw.github.com/jeffrwells/coderbase.io-staging/master/projects/photos/coderbase/coderbase-github-repo.png)

These files can now be edited any way that our users want. For quick changes, using the GitHub editor is often easiest. Users can also pull down the repository to edit it in their text editor or a special markdown program. Here you can see me writing these words (so meta) in Atom:

![Editing in Atom](https://raw.github.com/jeffrwells/coderbase.io-staging/master/projects/photos/coderbase/editing-in-atom.png)


Using markdown and a custom repository has huge advantages. You can see in the photo above that I created a photos folder to host my photos. Since we only parse the files that are properly named (.md files in posts or projects folder, _config.yml, etc.), I can put photos, notes, or other files in the repository if I would like.

As a developer, it is very easy for me to paste in code I want to show off. For example, here is how this file is formatted with YAMl at the top of this file:  

    ---
    title: Coderbase
    permalink: coderbase
    description: A portfolio & blogging platform for developers powered by git and markdown
    homepage: http://coderbase.io
    languages: [ruby, rails]
    visible: true
    order: 1
    ---

Settings are saved in a single YAML file: `_config.yml`. Here is mine, pasted in:

    # Your name to be displayed on your profile

    Name: Jeff Wells

    # The following three options specify
    # a madlibs-style description of yourself.
    # I ____________  at ____________________.
    # I like ________________________________.

    I: build things that make a difference
    at: @azmicrocredit, @coderbaseio
    I like: espresso, IPAs, cooking, fitness

    # Add your twitter handle to display your most
    # recent tweet and link to your twitter account.
    # Your twitter account is the only contact information
    # visible to the public.

    Twitter: @jeffrwells

    # Enter your Google+ id number to add an author meta tag to your posts.
    # Why? It allows Google to put your Google+ photo next to search results,
    # and generally ranks your posts higher on Google.
    # https://plus.google.com/authorship

    # Make sure to log in to Google+ and add 'www.coderbase.io' as a domain that
    # you contribute to, or it won't work.

    Google+: 112247879367781458018

    # Want your Coderbase.io profile to resolve on your own domain?
    # Enter your domain here e.g. mydomain.net or www.mydomain.net
    # and then create a CNAME record with your DNS provider
    # pointing to coderbase.herokuapp.com

    Domain: jeffrwells.com

    # You have the option to customize what you want to show on your main page.
    # By default the posts tab is selected. You can input:
    # 'posts', 'projects', or 'about'

    Homepage: posts


Also, everything is under version control without needing a service like PenFlip. Some of our users will use GitHub's pre-existing tools to Pull Request a typo fix for another user. Awesome!

Writing in the text editor in Markdown takes a little getting used to, but is very productive once you get the hang of it. It allows our users to focus on content and showing their abilities -- not getting bogged down with design or setup. All of this was created to work well for developers and make the process of creating a portfolio as smooth as possible.

In general, developers are not excellent designers. For those that are, they are best off making their own portfolio from scratch to showcase their skills in this area. Coderbase allows non-designers to have a well designed portfolio out of the box.

If you haven't already, you need to [try it out](https://coderbase.io). To see your changes instantly deployed from the command line is awesome!


# The Code

The code for this platform is hosted in a private repository, but here I will highlight some of the code I am proud of.
