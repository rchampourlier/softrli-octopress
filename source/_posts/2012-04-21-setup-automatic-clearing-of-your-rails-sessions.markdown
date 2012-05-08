---
layout: post
title: "Setup automatic clearing of your Rails sessions"
date: 2012-04-20 10:12
comments: true
categories: [rails, rubyonrails, sessions, session store, database, db, cleaning, cron, sysadmin, tips'n'tricks]
published: false

---
With every connection to your application of a new user, bot or whatever, Rails will create a new session object.

If you're using [ActiveRecord::SessionStore](http://api.rubyonrails.org/classes/ActiveRecord/SessionStore.html) to save your sessions in your database, your database may already be alarmingly growing in size, due to the number of sessions it contains.

## The rake db:sessions:clear task

If you don't mind a lot about your users lossing their session, you can simply use the provided `rake db:sessions:clear` task. It will simply delete all sessions in the database. Side-effect: all current sessions are deleted, so signed in users get signed out, and you should hope they had nothing under way!

## A more user-friendly solution


## References

* [http://blog.brightbox.co.uk/posts/clearing-out-rails-sessions](http://blog.brightbox.co.uk/posts/clearing-out-rails-sessions)