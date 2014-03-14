---
permalink: rails-asset-pipeline-with-bourbon-and-responcss
title: Rails asset pipeline with Bourbon and Responcss
languages: ruby, rails, css, scss
published: false
summary: My setup for customizing the Rails asset pipeline to effectively use SCSS.
---

Bourbon is a great library adding many sass mixins that can be used in your css files. It updates and deprecates mixins as modern browsers adopt these features and no longer need vendor prefixes.

[Responcss](http://responcss.com) is a great CSS framework that is simple to use and highly responsive. [(read here)](http://www.coderbase.io/jeffrwells/why-use-responcss)

Here is the final setup. Then I will go through and explain:
````


````

//application.css.scss
 *= require_self
// *= require_tree .
 */

@import "bourbon";
@import "lib/lib.css.scss";
@import "controllers/*.css.scss" ;


Now, the walkthrough

To set up  bourbon, add to gemfile:
//Gemfile
gem 'bourbon'

Rails asset pipeline by default compiles each file individually, which makes using SCSS variables and mixins nearly impossible. In order to fix this, change application.css to application.css.scss and remove the require_tree sprockets call.

//application.css.scss
 *= require_self
// *= require_tree .
 */

@import "bourbon";

Add @import “bourbon”. This will bring all of the bourbon mixins into this file, prior to compilation.

Next step is to bring in everything else that will use those mixins (really, all files because the require_tree is gone). Order is important!

I moved all of my stylesheets into a controllers folder, so they can be organized, and require all of them, after bourbon. Now these stylesheets will be concatenated on the application.css file, have access to bourbon mixins, and then be compiled.

//application.css.scss
@import "bourbon";
@import "controllers/*.css.scss" 



Now to add Responcss or other libraries. Downloading Responcss gives you a folder full of modular files, and a style.css.scss. This is useful because it makes it easy to change the variables in Responcss to customize it for your application.

Since most of my actual styles will be in base.css.scss or in the controller specific stylesheets, I changed this to media.css.scss and keep universal media queries in this file.

I then added a lib.css.scss file that will control everything in the lib folder.

@import "responcss/responcss.css.scss";
@import "media.css.scss";
@import "base.css.scss";


This file imports responcss.css.scss, which then imports all of the Responcss files. Then I grab the media queries in media.css.scss, and then bring in my base styles. base.css.scss will have access to everything in the folder responcss because of the order. I use this for base styles such as font, text-size, etc.

Finally, in application.css.scss, I just add @import "lib/lib.css.scss"; and now everything in the tree is taken care of, in correct order.

//application.css.scss

 *= require_self
 */

@import "bourbon";
@import "lib/lib.css.scss";
@import "controllers/*.css.scss";

Each controller stylesheet comes after everything in lib, so it will override the others. This way you won’t run into problems where you’re unable to override your library styles or variables.