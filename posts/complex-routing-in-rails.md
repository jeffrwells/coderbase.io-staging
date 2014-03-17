---
permalink: complex-routing-in-rails
title: Complex routing in Rails
languages:
summary: Forwarding an contraint matching in rails routing.
published: true
---

Working on the Coderbase.io platform, I wanted to make my resource urls as primary as possible, and avoid namespacing with /posts or /projects. This was an important design decision for us. Our system is controlled through a git repository that we create for the user, and posts are written in markdown.

Our file structure was like this:

    /
      /project1
        _config.yml
        post-about-project1.md
        post-2-about-project1.md
      /project2
        _config.yml
        post-about-project2.md
        post-2-about-project2.md
      _config.yml
      post-without-a-project.md
      another-post-without-a-project.md

The concept is that posts describe projects. Rather than sitting down and writing about your projects in detail, you write small posts as you go along.


It was important that our routes matched our file structure; we did not want to namespace our resources with `/projects` or `/posts`. We wanted routes like this:


    /username/project1
    /username/post-without-a-project
    /username/project1/post-about-project1

*NOTE: we have since pivoted away from this structure, but this is still a good example of our design constraints driving an interesting solution.*

This brought up a number of complications and we could not use the standard `resources :projects` implementation in Rails. There were two key issues to be resolved:

1. Both a project and a post can be a primary resource, so we need to allow this
2. Some posts are under a project and some are not, so we needed to forward the url for posts in a project


**The key to both of these solutions is that rails routes checks one at a time. If it doesn't match one, it looks on to the next.**


*(For these examples assume all permalinks are unique)*

## 1. Allowing two different resources to share a URL structure

In order to do this we need to list the two different resources and use contraints. If the url matches a project permalink -- we decided projects are more important than root posts -- then it will hit the project action on our controller. If not, it will try to match a post permalink, and hit the post action.

    scope '/:username' do
      #match project by permlaink
      get '/:permalink', to: 'profile#project' #constraints: 'Project exists?'
      #match post by permalink
      get '/:permalink', to: 'profile#post' #constraints: 'Post exists?'
    end

Constraints are created with a class instance that responds to `matches?`. These can exist either in the routes file or in a separate file (though the latter is better).

    class ProjectExistsConstraint
      def matches?(request)
        user = User.find_by(username: request.params[:username])
        user && user.projects.visible.exists?(permalink: request.params[:permalink])
      end
    end

    class PostExistsConstraint
      def matches?(request)
        user = User.find_by(username: request.params[:username])
        if user
          # we're allowing routes to unpublished posts if the user is logged in
          # we have the request object, so we check the session
          if user.id == request.session[:user_id]
            user.posts.exists?(permalink: request.params[:permalink])
          else
            user.posts.published.exists?(permalink: request.params[:permalink])
          end
        end
      end
    end

Now that we have the contraints we update the routes:

    scope '/:username' do
      get '/:permalink', to: 'profile#project', constraints: ProjectExistsConstraint.new
      get '/:permalink', to: 'profile#post', constraints: PostExistsConstraint.new
    end

We are done with this part. If a project can be found for the permalink, we send it to the project action. If not, it checks if there is a post with that permalink, and sends it to the post action. If neither, it's a 404 (or checks other routes).

You may want to use `find_by` instead of `exists?` in the constraints. The model should be cached and ready to use again in your controller. I went with the faster `exists?` query here so that I can do a more intricate eager loading query in the controller. It is also a bit more readable in my opinion.


## 2. Forwarding urls through the routes

Now we need to deal with our second complication. A user has seen multiple posts at

    /username/post-name

and wants to read *post-about-project1* and types in

    /username/post-about-project1

but get's a 404. We want to forward that automatically to

    /username/project1/post-about-project1


We can accomplish this by adding another route before the post matching route we made above. There is a `redirect()` method for redirection that takes params and request.

    scope '/:username' do
      get '/:permalink', to: 'profile#project', constraints: ProjectExistsConstraint.new
      # redirect if it is a post that actually belongs to a project
      get '/:permalink', to: redirect(status: 303) {|params,request| ProjectPostRedirector.url(params,request)},
                          constraints: ProjectForPostExistsConstraint.new
      get '/:permalink', to: 'profile#post', constraints: PostExistsConstraint.new
    end

The `constraints: ProjectForPostExistsConstraint.new` works just like the other constaint classes above. We use a **303 See Other** redirect because the url they tried to reach was formatted correctly, but we wan tthem to see the project in the url (to match the file structure).

To get the url for redirection is easy, we create a plain ruby class and give it a class method to find the url. Only trick is to include the url helpers:

    class ProjectPostRedirector
      # allow us to find project_post_url
      include Rails.application.routes.url_helpers

      def self.url(params,request)
        # We already used 'ProjectForPostExistsContraint',
        # so we know that the resources exists, we
        # just need find the url for it.

        # find user, post, and project
        # then use url helpers to get the route url
        Rails.application.routes.url_helpers.profile_project_post_url(user, repo.name, params[:permalink], request.params)
      end
    end

And there we go! These complicated routes can be tough to figure out but in all rails makes this much easier than it would be to do by hand.

There are a couple of potential conflicts that could arise from our url structure. *What if a project and post have the same permalink?* *What if a root post and project post have the same permalink?* We handled these in a clever way, but I will leave that for another post.
