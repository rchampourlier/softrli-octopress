---
layout: post
title: "Capybara using Selenium, with RSpec, in the Vagrant (remotely)"
date: 2012-11-08 12:00
comments: true
published: true
categories: [web, development, rubyonrails, testing, vagrant]

---

### Let's assume:

* you're using RSpec for your tests,
* you use Capybara for your integration/acceptance specs,
* you run your tests in a Vagrant box.

Let's also say everything is working fine (I won't deal with any issue you may have before this point).

Now, **you have some features / user stories / whatever that requires some Javascript action**.

### Well.

**If you weren't using Vagrant**, I'd say everything should go fine too. Just append `:js => true` to your RSpec example, it will use Selenium and everything will go fine using your installed Firefox binary.

But **you're testing in a Vagrant box**. So here's what you'll get:

```
Failure/Error: visit "/whatever"
     Selenium::WebDriver::Error::WebDriverError:
       Could not find Firefox binary (os=linux). Make sure Firefox is installed or set the path manually with Selenium::WebDriver::Firefox::Binary.path=
```

### So you may say, fine, let's install Firefox!

But... your Vagrant box is meant to be **used in headless mode**, and Firefox requires something to draw on. Still, you may have it to work. But I'll assume (again) that **you don't want to bother with setting things for the client-side of the test on the development/test box** (which is more server-side) **, and use everything that's already on you real development machine** (your day-to-day browsers).

_If my assumption is false, you can have a look to [this article](http://www.without-brains.net/blog/2012/08/01/capybara-and-selenium-with-vagrant/), which explains both approaches (but not for RSpec, that's the why of this article)._

### The solution? selenium server!

What you want is Capybara to use its selenium driver, but **instead of running a local copy of the browser**, use [selenium server](http://seleniumhq.org/download/) which will **allows Capybara to use browsers running in your real development machine** (the host of your Vagrant development box).

### Ok I got it, how do I do it?

[This article](http://www.without-brains.net/blog/2012/08/01/capybara-and-selenium-with-vagrant/) is pretty well made, with far more explanations that I provide, so I encourage you to have a look to fully understand how it works. However, **it doesn't provide the solution if you're using RSpec**, since it builds a simple `Unit::TestCase` example.

**This is where I truly do something.**

### Here's how to get it working with RSpec (and even in Spork)

Add this one line in your `spec_helper.rb` file (one line?!!!):

```
  require_relative 'support/capybara_remote'
```

After the `require 'capybara/rspec'` would be great. If you're using Spork, it should be in the `prefork` block.

Well, not truly a one-liner. Add this to a `spec/support/capybara_remote.rb` file too:

{% gist 4038197 %}

Be sure to adjust the constants (`SELENIUM_SERVER`, `SELENIUM_APP_HOST`, `CAPYBARA_DRIVER`) to match your configuration (as explained in the comments).

Now, get the selenium server `.jar` and run it on your real machine (the host of your Vagrant) with this command: 

```
java -jar selenium-server-standalone-2.25.0.jar 
```

Now, any RSpec example with the `:js => true` metadata should be running in your Firefox browser on your real machine, where you can see it!

### Wrapping up

We've seen how to use **selenium server** to allow **Capybara** running in a **Vagrant box** to use the host's **browsers** to run **RSpec** specs requiring **Javascript**.

### References

* [the main source for my custom configuration](http://www.without-brains.net/blog/2012/08/01/capybara-and-selenium-with-vagrant/)
* [more on how capybara allows to select a driver from the examples in RSpec](http://jeffkreeftmeijer.com/2011/capybara-ate-swinger/)