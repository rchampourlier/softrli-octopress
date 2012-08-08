---
layout: post
title: "Setup automatic clearing of your Rails sessions"
date: 2012-07-05 09:40
comments: true
published: true
categories: [development, sysadmin, rubyonrails, database, maintenance]

---
With every connection to your application of a new user, bot or whatever, Rails will create a new session object.

If you're using [ActiveRecord::SessionStore](http://api.rubyonrails.org/classes/ActiveRecord/SessionStore.html) to save your sessions in your database, your database may already be alarmingly growing in size, due to the number of sessions it contains.

### The rake db:sessions:clear task

If you don't mind a lot about your users loosing their session, you can simply use the provided `rake db:sessions:clear` task. It will simply delete all sessions in the database. Side-effect: all current sessions are deleted, so signed in users get signed out, and you should hope they had nothing under way!

### A more user-friendly solution

Writing a simple rake task to replace the default one and only remove sessions older than 2 weeks is as easy as adding this file to your project:

{% codeblock lib/tasks/sessions.rake lang:ruby %}
namespace :sessions do

  desc "Clear expired sessions (more than 2 weeks old)"
  task :cleanup => :environment do
    sql = "DELETE FROM sessions WHERE (updated_at < '#{Date.today - 2.weeks}')"
    ActiveRecord::Base.connection.execute(sql)
  end

end
{% endcodeblock %}

Now, if you run `rake sessions:cleanup`, only the older sessions will be removed from your database.

### Last but not least: automation

So that you don't have to run it every day, you can setup a cron task to do it for you. Here is the line we use:

{% codeblock crontab %}
# m h  dom mon dow   command
01 3 * * * bash -l -c 'cd /home/deployer/rails/project/current; bundle exec rake sessions:cleanup'
{% endcodeblock %}

### References

* [http://blog.brightbox.co.uk/posts/clearing-out-rails-sessions](http://blog.brightbox.co.uk/posts/clearing-out-rails-sessions)