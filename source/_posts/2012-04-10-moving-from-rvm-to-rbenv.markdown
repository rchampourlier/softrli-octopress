---
layout: post
title: "Moving from RVM to rbenv"
date: 2012-04-10 09:56
comments: true
categories: [sysadmin, ruby, gem, rvm, rbenv, gemset, dev, development]

---

* Using **rvm** to manage your gemset without really knowing why since now Bundler does it very cleverly?
* Feeling that the whole rvm system is a bit complicated?
* Tired of having shell scripts not finding your rvm-managed gems?

It looks like your ready to consider **rbenv**, which is a really lightweight alternative to **rvm**. Lightweight because it *just* manages rubies, and lets Bundler manage the gems.

<!-- more -->

## Why?

**RVM** is a very useful tool that helps you manage your ruby environment when you need multiple rubies and have tons of gems different for each of your projects.

I used **RVM** a long time on my development machine, however I did not bother to keep it on my production environment. Launching ruby applications through other ruby tools (e.g. God) or `crontab` was causing me headaches (loading RVM path, bundle not found, etc.). Since the gems were not managed by RVM, thanks to the excellent [Bundler](http://gembundler.com/), it was a lot of pain for a poor service.

On the other hand, **rbenv** is really simple and powerful, but it only manages rubies, not gems. But again, we have **Bundler**, so it's all that we need.

I will not say that using **rbenv** on a production server is painless, you still have to do some work to load some rbenv-things in your environment, but it's a lot more clear and easy than with **RVM**...

## Uninstall RVM

Start with the RVM auto-uninstall command:

    rvm implode
    
Check your filesystem for other `rvm` files which may remain: `.rvmrc` files, maybe `/etc/rvmrc`. Use `find / -name 'rvm' -print` or `locate rvm` for example.

## Install rbenv with ruby-build plugin

Install **rbenv** by following [the project's installation tutorial using GitHub checkout](https://github.com/sstephenson/rbenv#section_2.1). **Read the following before doing the install!**

Be sure to adjust the file to which you append your environments changes (steps 2 and 3) to the one your using on your system (`.bash_profile` in the tutorial, `.profile` on my Mac system, but you may have `.bashrc` too).

**Stop at the end of step 3**, **restart your term** (close and reopen, sure to work while step 4 `exec $SHELL` did nothing for me) and do the following to have a simple way of installing rubies:

    $ mkdir -p ~/.rbenv/plugins
    $ cd ~/.rbenv/plugins
    $ git clone git://github.com/sstephenson/ruby-build.git

If you've done this after step 4, you can just use this command to install the ruby you need, e.g. `1.9.2-p290`:

    rbenv install 1.9.2-p290

Don't forget to do step 6, `rbenv rehash`.

## Define rbenv's default ruby

Create the `~/.rbenv/default` file with the following content, adjusted to your desired ruby:

    ruby-1.9.2-p290
    
## Configure Bundler to manage gemsets

As a replacement to **RVM**'s gemsets feature, we will use **Bundler**. We will thus ask it to systematically install the gems inside the project's `vendor/bundle` directory, instead of using the system's default path.

To do this, create the `~/.bundle/config` file and add this content:

    ---
       BUNDLE_PATH: vendor/bundle
       
You can check the configuration is OK by `cd`-ing into one of your projects' directory (assuming it's using **Bundle** and so you have a **Gemfile**), and running `bundle config`. If the configuration is OK, you should have this showed:

    path
      Set for the current user (/Users/<your_name>/.bundle/config): "vendor/bundle"
      
## Using (and aliasing) bundle exec

Using Bundler to manage your gemsets will now require you to use `bundle exec` for **every** ruby command you run, even `rails server` or `rails console` (they don't need `bundle exec` to load Bundle, however you need Bundler to find them!).

A nice thing to do is to alias the `bundle exec` command to something shorter. I use `be`. Add this to your `~/.bash_profile`, `~/.profile` or whatever file you use:

    alias be='bundle exec'

So now you can run: `be rails console` instead of `bundle exec rails console`.

## The end

That's it, you're done!

## References / credits
This walkthrough is based on [this article](http://snippets.aktagon.com/snippets/532-How-to-migrate-from-rvm-to-rbenv) I used a lot myself, but I finally rewrote it to include some more details on the **rbenv** installation process.

## Hubticle tags

ruby, RVM, rbenv, gem, gemset, development, environment, bundle