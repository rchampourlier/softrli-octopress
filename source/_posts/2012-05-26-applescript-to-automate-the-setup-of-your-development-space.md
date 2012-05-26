---
layout: post
title: "AppleScript to automate the setup of your development space"
date: 2012-05-26 16:34
comments: true
categories: [development, mac, applescript]

---
Today is Saturday, and like every week-end, I'm getting a little lazy.

Say you're working on a Rails project on MacOS X. If you're like me, you like to have a terminal window for your project with several tabs:

* one for typing various commands,
* one with your `spork` process,
* one with your `rails console`,
* and one with your `rails server`.

Starting all this little world already involves some keystrokes and typing, even more if you're using Vagrant (which [I'm using for a week](http://www.softr.li/blog/categories/vagrant/) and is really awesome).

So why not use the simple tools provided by Apple to have this setup with a single action?

<!-- more -->

## The AppleScript

AppleScript is the scripting solution provided by Apple and it's perfect for what we want to do. It's quite simple, and it allows you to script the Terminal application.

Here is the script:

{% gist 2794125 %}

It's intended for my use, so it relies on some aliases I've defined on my host machine. But it's quite simple to read and comes with some comments, so I'm sure you will have no difficulty to adapt it to your needs.

## Access the script

You can run this script from a Terminal window by using `osascript path/to/the/script.scpt`, but I prefer to have it in the script menu.

You can activate the script menu from the script editor's preferences. Then just have a look in the menu to open the user's scripts directory and put your script file in it.

---

*If you need any help, just ask!*

*If you're aware of a better solution to setup a development environment like this, don't hesitate to tell me! I'm pretty sure there are some other ways I don't know yet...*