---
layout: post
title: "Using cloned virtualized machines but having issues with your network configuration?"
date: 2012-01-05 09:18
comments: true
categories: [virtualization, virtual box, issue, solution]

---
If like me, you're using **VirtualBox virtual machines** a lot, eg.:

* to **simulate your production environment** (what we may call *staging*),
* or more lately in my case, to **run a PostgreSQL database** and thus stop using SQLite to do your Rails developments and just use the production's server...

...then you may have wanted to clone some of your virtual machines once you installed the barebone server, and you may thus have faced **an issue with the network of the cloned machine not working anymore.**

**The solution to the problem is quite simple, but it needs to be found!**

This one is working well with my images under ** Ubuntu 10.04.3 LTS**. You can find some help [on this other post](http://blog.computerant.com/2010/01/02/virtualbox-cloning-ubuntu/), or if you want the shortest way:

```
sudo mv /etc/udev/rules.d/70-persistent-net.rules /etc/udev/rules.d/70-persistent-net.rules.broken
sudo shutdown now -r
```

This should restart your machine... and restore your network! Have fun ;)