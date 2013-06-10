---
title: "Easy environment specific seed files in Rails 3"
date: 2010-09-30 02:36
comments: true
tags: Rails
---

Tonight I discovered a [lighthouse ticket](https://rails.lighthouseapp.com/projects/8994/tickets/4908-feature-request-db-seed-files-for-each-environment) by [Postmodern](http://houseofpostmodern.wordpress.com/). In it, he asks about adding an environment aware feature to the existing 'rake db:seed' task, so that he could seed specific environments differently. He even includes [a patch](http://github.com/postmodern/rails/commit/7dd0718239c6747e1a6981aed9b9c406532e9828).

If you haven't used rails 'rake db:seed' functionality, its quite handy. I use it primarily during the pre-launch phase of development to fill out my app with dummy data, most of which is generated using the [ffaker](http://github.com/EmmanuelOga/ffaker) gem. That all works fine for me!

Up until ... the time that my client wants to see progress and I need to deploy my code to a staging server. For that I require an entirely different set of seed data. Try as I might, I just can't get the client used to my beloved "Lorem ipsum dolor". So, environment aware seeding is very useful to me.

A neat thing about rake tasks is that when you redeclare a task, it doesnt overwrite the previous one. So, to use postmodern's patch all you have to do is paste the following code to the bottom of your Rakefile and you'll reap the full benefits.

~~~ ruby
namespace :db do
  task :seed => :environment do
    env_seed_file = File.join(Rails.root, 'db', 'seeds', "#{Rails.env}.rb")
    load(env_seed_file) if File.exist?(env_seed_file)
  end
end
~~~

Now you can do stuff like this! (the env specific file is loaded after the regular seeds file)

~~~
db
+-- seeds
|   +-- development.rb
|   +-- production.rb
|   +-- staging.rb
+-- seeds.rb
~~~

Thanks Postmodern

