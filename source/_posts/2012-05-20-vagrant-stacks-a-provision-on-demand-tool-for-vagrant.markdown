---
layout: post
title: "vagrant-stacks, a provision-on-demand tool for Vagrant!"
date: 2012-05-20 09:42
comments: true
categories: [vagrant, chef, development, sysadmin, maintenance, unix, opensource]

---
[Vagrant](http://www.vagrantup.com) is really an excellent tool for building and using virtual machines for your development environment. It **ease the maintenance** and allows to **share the same environment within the whole team**.

However, **Vagrant itself only provides you with barebones boxes**.  You still have to work for provisioning the box, **which requires that you build a Chef recipe** (or Puppet, but I'm using Chef so I will only refer to this one).

**Instead of using a packaged box** (e.g. from [vagrantbox.es]([http://vagrantbox.es)) which you may **not trust or feal comfortable with**, you can use [this repo](https://github.com/rchampourlier/vagrant-stacks) to **build and package your Vagrant box by yourself**. It provides the tools and some configurations which will enable you to build a provisioned box without writing the recipe! The advantage of this approach over packaged boxes is that you can **review how the box is built and provisioned**. You can even refine the configurations and build new ones by forking the repo and submitting pull requests!

It already provides a configuration for a **Ruby 1.9.2-p290 stack, managed with rbenv and Bundler, plus a PostgreSQL 8.4 database and MongoDB**.

So feel free to **try and share new configurations**! Enjoy!