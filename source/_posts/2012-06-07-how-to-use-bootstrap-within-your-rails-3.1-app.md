---
layout: post
title: "How to use Bootstrap within your Rails 3.1+ app"
date: 2012-06-07 12:00
comments: true
categories: [development, rubyonrails, webdesign]

---

[Bootstrap](http://twitter.github.com/bootstrap) is a real gift from Twitter. It provides a good deal of packaged, well-designed and simple to use UI components for any web application.

I wanted to use it for quite some time, but I hadn't found the time nor the project to apply it on. I recently updated an administration web-app for one of my project, and I felt it was the good time.

So here are my first feedbacks and tips on how to integrated Bootstrap with Rails (I'm on Rails 3.2.5, it's the lowest-effort path if you're using Formtastic too).

<!-- more -->

### First, where do I put the files?

Say you want to use [Twitter Bootstrap](http://twitter.github.com/bootstrap) within your Rails 3.1+ (ie. asset pipeline featured) application.

After some tests I couldn't find a simple way to **keep the whole bootstrap directory packaged and get it accessed correctly by the asset pipeline**. So the best way I found yet is to **explode it into the appropriate directories**:

- CSS files goes into `vendor/assets/stylesheets`
- JS files goes into `vendor/assets/javascripts`
- image files goes into `vendor/assets/images`

So this way I can simply require Bootstrap CSS through this `application.css` file:

```css app/assets/application.css
/*
 * This is a manifest file that'll automatically include all the stylesheets available in this directory
 * and any sub-directories. You're free to add application-wide styles to this file and they'll appear at
 * the top of the compiled file, but it's generally better to create a new file per style scope.
 *= require_self
 *= require_tree . 
 *= require bootstrap
*/
```

If this does not work, be sure to **restart your development server** as changing location of assets may not be taken into account until restart...

### This breaks my tasty forms!

Are you using [Formtastic](https://github.com/justinfrench/formtastic)? I am, this is one of the things I like to have with every Rails project. However, Formtastic and Bootstrap do not naturally go together well.

To make them love each other, we have to add a gem which will tweak the way Formtastic builds the forms to make them Bootstrap-proof. This is the intent of [formtastic-bootstrap](https://github.com/rchampourlier/formtastic-bootstrap).

You can add this to your Gemfile, that should be all:

```
gem 'formtastic-bootstrap', :git => 'https://github.com/rchampourlier/formtastic-bootstrap.git', :branch => 'bootstrap2-rails3-2-formtastic-2-2-1'
```
_This assumes your on Rails 3.2 and Formtastic 2.2.1, as the branch name indicates._

I've sent you to my own fork which is forking an already long chain of forks. The [original repo](https://github.com/mjbellantoni/formtastic-bootstrap) seems not to be maintained anymore. My fork provides a `bootstrap2-rails3-2-formtastic-2-2-1` branch which should work for Rails 3.2 and the current version of Formtastic, the 2.2.1 (some changes were to be taken into account from 2.2.0).

_My fork does not take all 2.2.1 changes into account. You may have issues with `time_select` inputs..._

### My flash messages aren't that nice!

To style your Rails' flash messages using the nice Bootstrap's alert style, you can have a look to this [gist](https://gist.github.com/2887844):

{% gist 2887844 %}

Just replace your current way of displaying the flashes in your template with this, and you're done!

```ruby
= render :partial => 'layouts/flash', :locals => { :flash => flash }
```

---

_I hope this helps, do not hesitate if you see mistakes or some parts which require additional details!_