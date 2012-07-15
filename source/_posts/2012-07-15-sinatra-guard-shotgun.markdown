---
layout: post
title: "Faster Sinatra development with guard-shotgun"
date: 2012-07-15 18:20
comments: true
published: true
categories: [development, ruby, sinatra, opensource, projet]

---
[Sinatra](http://www.sinatrarb.com/) is a really nice way to **bootstrap projects**. It allows to **start really fast**, and is really suited for **API-based** projects (may be a good choice for your next mobile-or-web-app backend).

Unlike Ruby on Rails however, Sinatra does not reload your code with each request, so you have to restart the server each time your code changes. To help you with this burden, there is a small tool called [Shotgun](https://github.com/rtomayko/shotgun).

I've used it but I rapidly faced a [logging issue](https://github.com/rtomayko/shotgun/issues/34) making it useless.

Since I was using **the excellent [Guard](https://github.com/guard/guard/)**, and I had already seen a similar reloading module for Webrick, I just decided to code [guard-shotgun](https://github.com/rchampourlier/guard-shotgun). It's a fairly simple **plugin for Guard** providing the **same feature as Shotgun**, but using the rock-solid fundation provided by Guard!

Feel free to use, fork, and even feedback it!