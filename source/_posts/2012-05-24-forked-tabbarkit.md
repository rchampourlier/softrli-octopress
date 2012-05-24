---
layout: post
title: "Forked TabBarKit"
date: 2012-05-24 16:52
comments: true
categories: [development, ios, opensource]

---
I have had to rewrite some parts of TBKTabBarController since I needed to be able to programmatically select a tab. This is normally possible by setting the `selectedIndex` or the `selectedViewController` property of the tab bar controller, but this was not working with TabBarKit.

I investigated and detected this behavior was not implemented, so I decided to complete it myself.

Along the way, I discovered there were some issues with the `containerView` being mixed up with the `TBKTabBarController`'s view, so I also corrected these bugs.

I've forked the repository and updated it with my corrected code, so have a look to this [repo](https://github.com/rchampourlier/TabBarKit) if you need this.

Oh, I also added a `TBKTabBarStyleTwiceHeight` I use to double the size of the tab bar on iPad, so you get this for free too ;)