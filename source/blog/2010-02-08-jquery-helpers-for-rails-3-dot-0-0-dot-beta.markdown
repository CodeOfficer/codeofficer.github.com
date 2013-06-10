---
title: "jQuery helpers for Rails 3.0.0.beta"
date: 2010-02-08 05:07
comments: true
tags: Javascript, Rails
---

I've just ported the unobtrusive javascript helpers in Rails 3 from Prototype to jQuery-1.4.1, but I haven't tested it to work with earlier versions of jQuery yet. Let me know if you find this useful or have any issues installing the gem.

For those that don't know, Rails 3 now makes use of HTML5 custom data attributes on the DOM instead of generating javascript inline when using it's built in javascript helpers. If you're not planning to use HTML5 in your app, you might benefit from another plugin I wrote called [js-data-helper](http://github.com/CodeOfficer/js-data-helper).

Thanks to [@technicalpickles](http://technicalpickles.com/) for helping me package this up as a gem.

[The code is on Github](http://github.com/CodeOfficer/jquery-helpers-for-rails3)

[The gem is on Gemcutter](http://gemcutter.org/gems/jquery_helpers)

~~~
Install:
  (sudo) gem install jquery_helpers

Usage:
  rails generate jquery_helpers  [options]

Runtime options:
  -f, [--force]    # Overwrite files that already exist
  -p, [--pretend]  # Run but do not make any changes
  -q, [--quiet]    # Supress status output
  -s, [--skip]     # Skip files that already exist

Description:
  A port of the unobtrusive Prototype helpers in Rails 3 to jQuery

Example:
  ./script/generate jquery_helpers

This will create:
  public/javascripts/jquery.rails.js
  public/javascripts/jquery.rails.min.js
~~~

Don't forget to include 'jquery.rails.js' in your layout along with jQuery.

UPDATE May 11, 2010:
Rails has their own port to jQuery that is better maintained now.
[http://github.com/rails/jquery-ujs](http://github.com/rails/jquery-ujs)

