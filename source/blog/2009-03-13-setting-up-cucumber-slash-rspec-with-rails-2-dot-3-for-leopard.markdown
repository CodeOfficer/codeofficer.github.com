---
title: "Setting up Cucumber/RSpec with Rails 2.3 for Leopard"
date: 2009-03-13 02:54
comments: true
categories: Rails
---

Install the gems:

~~~ bash
sudo gem install rails --source http://gems.rubyonrails.org
sudo gem install dchelimsky-rspec
sudo gem install dchelimsky-rspec-rails
sudo gem install cucumber
sudo gem install webrat
~~~

Create your app:

~~~ bash
rails myapp
cd myapp
~~~

Freeze Rails to vendor:

~~~ bash
rake rails:freeze:edge
~~~

Manage your gem dependencies through rails, edit config/environments/test.rb. Add the following lines:

~~~ ruby
config.gem "dchelimsky-rspec", :lib => false, :version => ">= 1.1.99.13"
config.gem "dchelimsky-rspec-rails", :lib => false, :version => ">= 1.1.99.13"
config.gem 'aslakhellesoy-cucumber', :lib => false, :version => ">= 0.1.99.23"
config.gem 'webrat', :lib => false
~~~

Unpack your gems:

~~~ bash
rake gems:unpack RAILS_ENV=test
rake gems:unpack:dependencies RAILS_ENV=test
~~~

And generate your testing folders:

~~~ bash
script/generate rspec
script/generate cucumber
~~~

That's it, have fun.

