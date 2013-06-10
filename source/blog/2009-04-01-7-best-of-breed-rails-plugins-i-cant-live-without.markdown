---
title: "7 Best of Breed Rails Plugins I Can't Live Without"
date: 2009-04-01 02:27
comments: true
tags: Rails
---

I am often asked about the various gems/plugins I use in my Rails apps and thought it might make for a good blog post. Most of these I have a good history with, while some I settled on only recently. Either way, this list contains what I consider to be the best of the best, for what each of them helps to accomplish. I hope you will check them out if you have not yet had a chance to.

##REVISIONING

[acts_as_revisable](http://github.com/rich/acts_as_revisable/) ([rich](http://github.com/rich))
A framework for building heavily versioned applications.

acts_as_revisable is a joy to use. It doesn't have you create separate tables for versions like some other popular versioning plugins. The interface is very well thought out, and I've yet to find something it doesn't do that I've needed. Its heavily documented and just works!

##TAGGING

[acts-as-taggable-on](http://github.com/mbleigh/acts-as-taggable-on/) ([mbleigh](http://github.com/mbleigh))
A tagging plugin for Rails applications that allows for custom tagging along dynamic contexts.

A tried various tagging solutions over the last year and finally settled on this one by mbleigh. acts-as-taggable-on-steroids and a few others never seemed to work on models build with single table inheritance (STI) or that had polymorphic relationships.

##AUTHENTICATION

[authlogic](http://github.com/binarylogic/authlogic/) ([binarylogic](http://github.com/binarylogic))
A clean, simple, and unobtrusive ruby authentication solution.

Like most of the Rails community, I earned my stripes using Restful Authentication, a sexy authentication generator that got you up and running in no time at all, but it wasn't long after that I began to find things I didn't like about it. Overall, Restful Authentication felt very non-object oriented. It polluted my user model with code I hadn't written and it didn't even feel restful. Authlogic on the other hand makes no assumptions on how you'd like to authenticate users. It lets you treat your UserSession as a model and take advantage of ActiveRecord's build in callbacks. Its also a Gem and easy to update. Nowadays you won't find me using anything else. Its also heavily documented.

##BACKGROUND JOBS

[delayed_job](http://github.com/tobi/delayed_job/) ([tobi](http://github.com/tobi))
Database backed asynchronous priority queue—Extracted from Shopify.

I don’t have a lot of experience with background process plugins, I’ll admit that up front. Hands down though, this one has some slick features and is very actively developed. No complaints.

##GIT WORKFLOW

[git_remote_branch](http://github.com/webmat/git_remote_branch/) ([webmat](http://github.com/webmat))
A tool to simplify working with remote branches.

If you are a frequent brancher, and need to sync local branches with your remote server there is no better way than using git_remote_branch. You can publish and track local branches in a single command. I find myself doing this a lot when I have created a branch on my laptop but want to continue where I left off on my other machine at home. It also has a helpful ‘explain’ feature to tell you exactly which commands will run should you use one of its features.

##GEM BUILDING

[jeweler](http://github.com/technicalpickles/jeweler/) ([technicalpickles](http://github.com/technicalpickles))
Simple and opinionated helper for creating and managing Rubygem projects on GitHub.

I knew nothing about writing gems, and pushing them to github ... 20 minutes later I had. Nuff said.

##STATE MACHINES

[state_machine](http://github.com/pluginaweek/state_machine/) ([pluginaweek](http://github.com/pluginaweek))
Adds support for creating state machines for attributes on any Ruby class.

This one I found by a [friend’s recommendation](http://heypanda.com/). I had been using Rubyist’s Acts As State Machine (now known as AASM) and had many issues with it early on. Specifically I had been trying to cancel a transition in a callback and eventually found out it wasn’t supported. My understanding is that this plugin was originally built for Rails and later refactored to to be less dependent on it. state_machine however was built from day one to work with any Ruby class, and later integrations were added for ActiveRecord, Datamapper and Sequel. Its the best thing out there and I can’t recommend it enough. The code is a joy to look though and its heavily documented.
