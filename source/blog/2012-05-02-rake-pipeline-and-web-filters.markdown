---
title: "Rake-Pipeline and Web Filters"
date: 2012-05-02 02:36
comments: true
tags: Ruby Javascript
published: false
---

[Rake-Pipeline](https://github.com/livingsocial/rake-pipeline) is an asset packing tool used primarily in the Ember.js community. It's similar to Sprockets or Jammit but more lightweight. [Rake-Pipeline-Web-filters](https://github.com/wycats/rake-pipeline-web-filters) expands the default set of filters that can be used with Rake-Pipeline. These projects are not yet well documented, but they are easy enough to get started with.

Rake-Pipeline works by evaluating an `Assetfile` in your project's root folder. The `Assetfile`, written in ruby, uses a DSL to express one or more pipelines and their output. A pipeline begins with an input statement which accepts a path and a regular expression. All Files matched by the regular expression within the path will yield to the filters you've specified, and the result will go into your output directory. Here's an example Assetfile:

~~~ ruby
require "rake-pipeline-web-filters"
output 'www'

input "app/vendor", "**/*.js" do
  filter(Rake::Pipeline::OrderingConcatFilter, [
    'minispade.js',
    'jquery.js',
    'ember.js'
  ], 'vendor.js')
end
~~~

Here we're matching all `.js` files within our project's `app/vendor` folder. We'll order them by the files we've listed first, then append the rest and concatenate them to a single file called `vendor.js` which will then get written to our output folder. Its that easy!

There are quite a variety of filters available and they can be combined in various ways.

 Filters in Rake-Pipeline:

* ConcatFilter - concatenates files
* OrderingConcatFilter - concatenates files in a specified order
* PipelineFinalizingFilter - copies generated files over to its output

 Filters in Rake-Pipeline-Web-Filters:

* CacheBusterFilter - inserts a cache-busting key into the outputted file name
* ChainedFilter - enable filters to be applied to files based upon their file extensions
* CoffeeScriptFilter - compiles CoffeeScript to JavaScript
* HandlebarsFilter - converts handlebars templates to javascript and stores them in a defined variable
* LessFilter - compiles each file with Less.js
* MarkdownFilter - compiles each Markdown file using the Redcarpet compiler
* MinispadeFilter
* SassFilter
* TiltFilter
* UglifyFilter
* YUICssFilter
* YUIJavaScriptFilter


https://github.com/dockyard/ember-jasmine-standalone


By specifying a series of inputs and filters to use and the result is written to your output folder.

is written in a DSL which may specify a series of inputs, filters to run on those inputs, and outputs.


It will apply filters to a number of
directory inputs and give you . The easiest way to get
started is:

Step 1: Install bundler
$ sudo gem install bundler

Step 2: Create a Gemfile
$ bundle init

Step 3: Add rake-pipeline to the Gemfile
gem 'rake-pipeline-web-filters'

Step 4: Install your gems
$ bundle install

Step 5: Set up Assetfile

Step 6: Run Rake::Pipeline
$ bundle exec rakep
$ bundle exec rakep build



An extension to Rake for dealing with a directory of inputs, a number of filters, and a directory of outputs



~~~ ruby
def sleep_for(sleep_time, message = 'Sleeping...')
  sleep_time.times do |i|
    print_message = "#{message} #{sleep_time - i} seconds remaining"
    print print_message
    sleep 1
    print ["\b", " ", "\b"].map { |c| c * print_message.length }.join
  end
end
~~~
