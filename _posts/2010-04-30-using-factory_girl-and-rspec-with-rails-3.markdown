---
layout: post
title: "Using factory_girl and Rspec with Rails 3"
published: true
---

**Edit**: As Wes pointed out, the problems I was having were related to using the beta's of Rails, Rspec and Factory Girl. These are all resolved now, and the only trick is to make sure you use the [Factory Girl Rails](https://github.com/thoughtbot/factory_girl_rails) gem. Luckily that is as easy as dropping...

    gem 'factory_girl_rails'` 

...in to your Gemfile. Thanks Wes.

-----

Just like anything in Rails 3, start by editing my Gemfile to include Rspec and Factory Girl:

    group :test do
      gem "rspec-rails", ">= 2.0.0.beta.8"
      gem 'factory_girl', :git => 'git://github.com/thoughtbot/factory_girl.git', :branch => 'rails3'
    end

Then following the factory_girl documentation I created a new user factory in `spec/factories` but got hit with an unexpected error while factory_girl was running her `find_definitions` step:

    /spec/factories/users.rb:1: uninitialized constant User (NameError)

The solution was simple. I noticed in the generated spec_helper Rspec was loading everything nested under `spec/support`. So I simply moved my factories directory to `spec/support/factories` and I saw the sweet red dots of failing tests.

Time to go make them specs pass.
