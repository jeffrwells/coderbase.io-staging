---
title: Coderbase
permalink: coderbase
description: A portfolio & blogging platform for developers powered by git and markdown
homepage: http://coderbase.io
languages: [ruby, rails]
visible: true
order: 1
---

*Note: This project is very much a work in progress. This portfolio was created on, and is hosted by, Coderbase.io.*

Coderbase.io is a portfolio platform for developers. It seeks to create the ideal online identity for developers. It combines blog-style posts, projects that contain annotations -- such as what you are reading right now -- and basic pages.

One of the most interesting parts about it is that everything is driven through Markdown, YAML, and Git. There is no admin UI through the browser. When a user signs up through GitHub, a new repository is created on his/her GitHub account that contains all of the configurations and content.

See below for further details and pictures. *Lots of code samples*


# Motivation

Hiring extremely talented software engineers is a significant challenge for tech companies today. More than any other factor, the people at a company make or break its success. For this reason, managers and recruiters go to great lengths to determine a job candidate's abilities and qualifications before making a decision. Time and effort are wasted for the employer as well as the engineer.

To attack this problem my team and I wanted to create a platform that allowed developers to show their proficiency in programming, their personality, and their ability to solve problems as well as gain exposure through writing.

We hope to replace a resume, blog, and GitHub profile with one single portfolio accessible by a custom domain name: (eg jeffrwells.com). The portfolio is well designed and minimally featured to make it easier to use and more attractive than any other service or custom-built solution.


# How it works

Coderbase.io is built for developers, and is designed to work in tune with a developer's typical workflow. We recognized early on that the context switch required -- switching from a text editor to the browser, logging in, writing a post, and publishing it all in one sitting -- would be extremely prohibitive. We created a simpler workflow using Git and Markdown.

When a user signs up through his/her GitHub account, a new repository called *coderbase.io* is created that hosts all of the portfolio's posts, projects, and configurations in markdown and YAML files. When the repository is modified and pushed to GitHub, a commit hook sends the changes over to us and we parse it and update the portfolio accordingly.

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

I am going to walk through some of the background details of this platform, from a user signing up to how posts and projects are updated.

## Customizing YAML with StringyYAML

We embed YAML frontmatter in our markdown files like I demonstrated above. When a user signs up we create a new Markdown file for each of his/her GitHub projects. In order to do this, we not only customize the name of the file, but also the values of all of the metadata keys in YAML.

Ruby's Psych YAML library is good for parsing or writing pure YAML files, but is not built to deal with YAML inside of a string (which this file is until committed). I created a StringyYAML class that uses YAML but treats it like a string but escapes everything, and allows for updating the values by passing in a hash.

    require 'yaml'

    module StringyYAML
      include YAML

      class << self
        # load to convert everything to strings
        def load(content)
          # escape double quotes
          content.gsub! /\"/, '\"'
          # wrap everything in double quotes
          content.gsub! /^(.+?): (.*)/, '\1: "\2"'
          YAML.safe_load(content)
          # WYSIWYG!
        end # end load

        def update(string,first,second=nil)
          str = string.dup
          if !second.nil?
            key,value = first, second
            replace!(str,key,value)
          else
            hash = first
            hash.each_pair do |key,value|
              replace!(str,key,value)
            end #end each_pair
          end #end if/else
          return str
        end

        private
        def replace!(string, key, value)
          string.gsub!(/^#{key}:(.+)/) do |match|
            # match: String 'key: old_value'
            # $1: String 'old_value'
            match.gsub($1," #{value}" || ' ')
          end
        end

      end
    end

This class has two components.

The first is that we wanted our values to be parseable even if they were misformatted. For example, if a user provided...

    ---
    summary: ['this','that']
    ---

...the value that would be saved would be the string `"['this','that']"`. Natively, YAML would parse this as an array, and would create a `TypeError` in the application when `summary` expected a string. By escaping all double quotes, and then wrapping all values in quotes (as you can see in the RegExp above) we allow our YAML files to have a What You See Is What You Get response, and eliminate errors whenever possible. This avoids errors with single quotes or colons in a value as well, and values starting with symbols such as `Twitter: @jeffrwells` do not need to be wrapped in quotes. Integer, boolean and array values can be converted from a string when necessary.

The second component is the `update` method. This allows us to take a template of the YAML data we will need and update it with the proper values for the user. For example, when a user signs up, we create a project with each repository from GitHub. Here is an excerpt of how we do that:

    class CustomSeedRepoBuilder

      def initialize(user)
        @user = user
        @repos = user.repos
      end

      def files
        # customize user config...

        # customize repos
        @repos.each_with_index do |repo, index|
          @files["#{repo.filename}"] = merge_project(repo)
        end
        # ...
      end

    private

      def merge_project(repo)
        StringyYAML.update(
          SeedRepoTemplate.project,
          {
            title: repo.title,
            permalink: repo.permalink,
            description: repo.description,
            homepage: repo.homepage,
            visible: repo.visible,
            languages: "[#{repo.languages.join(', ')}]"
          }
        ) << "\n#{repo.readme_markdown}"
      end

    end

You can see that we pass the original string that we use as the template -- which is accessed with a class method on SeedRepoTemplate -- and update it by passing it a hash with the new values. We then copy exactly the README contents of the repository as a basis for a description like you are reading now.

*As a note, we never expose contents of private repositories. The `repo.visible` value defaults to false for all private or collaborator repos.*


## Parsing Commits

This is an extensive process but I will show the key files that make this happen.

When the user signs up we create a webhook on their new repository that sends commit data over to the application. When a commit comes in, we have the commit sha and url, along with some other data. Here is the trail it follows as it is processed:

### Holding the commit information

The first thing is to hold this commit information somewhere. The `CommitInformation` class parses the user information out of the commit url and has lazy instantiators for the data we might need (since the data needed will change depending on the commit and files affected). An instance of this class gets passed along as needed.

    class CommitInformation

      COMMIT_URL_REGEX = /.+\/(.+)\/(.+)(\/commit\/|\/git\/)/

      def initialize(commit_sha, commit_url)
        @commit_sha = commit_sha
        @commit_url = commit_url

        @commit_url.match COMMIT_URL_REGEX
        @github_username = $1
        @github_repo_name = $2
      end

      def files
        @files ||= commit_obj.files
      end

      def conn
        @conn ||= GithubConnection.new(user)
      end

      def commit_obj
        # returns github gem commit object
        @commit_obj ||= conn.get_commit(@commit_sha)
      end

      def tree_obj
        # returns github gem tree object
        @tree_obj ||= conn.get_tree(tree_sha)
      end

      def tree_sha
        @tree_sha ||= commit_obj.commit.tree.sha
      end

      def user
        @user ||= User.find_by(username: @github_username)
      end
    end

The user can be found from the unique username in the commit url, and GithubConnection is our client for connecting to the Github API.

### Sending the modified file to the right processing class

Now that we can hold the information, we need to parse the files that are changed.

    class CommitHookParser

      def initialize(commit_sha, commit_url)
        @commit_info = CommitInformation.new(commit_sha,commit_url)
        self
      end

      def parse_commit!
        @commit_info.files.each do |file|
          match_file_name(file)
        end

        self
      end


    private

      def match_file_name(file)
        case file.filename
          when POST_FILENAME_REGEX
            PostProcessor.process!(@commit_info, file)
          when PROJECT_FILENAME_REGEX
            ProjectProcessor.process!(@commit_info, file)
          when CONFIG_FILENAME_REGEX
            ConfigProcessor.process!(@commit_info, file)
          when ABOUTME_FILENAME_REGEX
            PageProcessor.process!(@commit_info, file)
          else
            # nothing with other kinds of files
        end
      end

    end

After instantiating `CommitInformation`, it matches each file to the proper processer based on the file type.

### Processing the commit data

Here is what a processor looks like:

    class PostProcessor

      attr_accessor :file, :name, :commit

      # class method is just a wrapper for instantiating the object
      def self.process!(commit_info, file)
        self.new(commit_info, file).process_file!
      end

      def initialize(commit_info, file)
        @file = file
        @commit_info = commit_info
      end

      def process_file!
          case @file.status
            when 'modified','added'
              process_post
            when 'renamed'
              rename_post
            when 'removed'
              remove_post
          end

          self
      end


      def process_post
        post = @commit.user.posts.find_or_initialize_by(filename: @file.filename)
        post.fetch_and_update!(@file.sha)
      end

      # rename_post, remove_post, etc...

    end

Simple enough. It determines the action based on the git status of the file, and in this case, create or update the post. The responsibility of fetching the blob for the post and parsing it belongs to the `Post` class.

### Fetching and updating

First it uses the sha to fetch and decode the blob file from the GitHub API. `user.github` holds the API client and provides convenient methods like `get_blob`.

    def fetch_and_update!(sha)
      blob = user.github.get_blob(sha)
      content = Base64.decode64(blob.body.content)
      update_from_yaml!(content)
    end

Then the content is passed to `parse_yaml`, where the values are extracted and returned as a hash. Then the Post model's values are individually updated from the hash. Finally it is saved, and we don't expect any errors.

    def update_from_yaml!(content)
      data = parse_yaml(content)
      #parse_yaml sets markdown, markdown is parsed in before_save
      if data
        self.title = data['title']
        self.languages = data['languages'] || []
        self.published = data['published'] || true
        # etc
        save!
      end
    end


Here is a look at the `parse_yaml` method. It matches the content of the post to our YAML frontmatter regex, and sets the markdown and yaml accordingly. StringyYAML is used to load the values as pure strings in the WYSIWYG method I described above.

    def parse_yaml(content)
      if content =~ /\A(---\s*\n.*?\n?)^(---\s*$\n?)/m
        yaml = $1
        self.markdown = $POSTMATCH

        StringyYAML.load(yaml) rescue psych_syntax_error
      else
        self.markdown = ""
        misformat_error
      end

    end

But what happens if the post isn't correctly formatted? Since Coderbase.io does not have any administrative tools in the browser, and we cannot display an error in their text editor, we use a unique method. The `psych_syntax_error` and `misformat_error` methods add a code block onto the beginning of the post's content that describes that there was an error and sets the post to a draft. Then when the user goes to look at the post, they see the error displayed at the top. We've considered many options for notifying errors, but at present this is the smoothest. We do not want to create notifications or admin interfaces unless we absolutely have to, as it is very important that this tool works seamlessly into our users' current workflow.

That is pretty much how the parsing works. Obviously there are edge cases and a lot more detail under the surface. All of this was test driven with RSpec. Tests are extremely important for this as it is difficult and slow to QA test and it is something that must work perfectly.

Hopefully these were valuable code samples to see my coding style and abilities!
