---
layout: post
title: "How to make your own gems with Jeweler"
date: 2012-04-14 10:03
comments: true
categories: [development, ruby]

---
A really simple walkthrough on how to **package a gem easily** using the **Jeweler** gem.

<!-- more -->

## Prerequisites

* You're using Bundler.
* You have `gem 'jeweler'` in your `Gemfile`.
* You have run `bundle install`.

## Add the `gemspec` task to your `Rakefile`

Add these lines to your `Rakefile`. This will help Jeweler to build the appropriate `gemspec` for your gem.

    begin
      require 'jeweler'
      Jeweler::Tasks.new do |gemspec|
        gemspec.name = "the_name_of_your_gem"
        gemspec.summary = "A summary for your gem"
        gemspec.description = "A description for your gem"
        gemspec.email = "your_name@your_company.com"
        gemspec.homepage = "http://an_url_for_your_gem.com"
        gemspec.authors = ["your@email.com"]
      end
    rescue LoadError
      puts "Jeweler not available. Install it with: gem install jeweler"
    end
    
## Package the gem

To package the gem, you just have to run the `rake` task you added, and another `build` task.

    rake gemspec
    rake build

## Include your gem hosted on Github in your Rails application

Assuming your repository is commited on Github, you can easily include your gem in your Rails project using the following line in your `Gemfile`:

    gem 'your_gem_name', :git => "path_to_your_gem.git"
    
You can also specify a branch with `:branch => "stable"` and a tag with `:tag => "2-stable"`.

## Include your gem from a local directory

You can tell Bundler to get your gem from a local directory using the `:path` parameter. The path is relative to the directory containing the `Gemfile`:

    gem "rails", :path => "vendor/rails"
    
## Directly include the library

_If you're building a gem, that's probably not your goal, but you can also just include an external lib without needing to package it as a gem. Just use:_

    require 'path_to_gem/lib/gem'

## Hubticle tags

ruby, gem, jeweler, library, module, share, github
