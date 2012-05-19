---
layout: post
title: "Setting up log rotation for your web apps"
date: 2012-04-20 10:12
comments: true
categories: [sysadmin, maintenance, unix]

---
{% pullquote %}
If, like me, you simply follow the common tutorials and walthroughs to setup your production environment, you may start wondering after some weeks **where this leak on your server's disk space is coming from...**

{" Just ask yourself this question: did you configure your logs rotation? "}

*(Well, that was when I was still a young CTO...)*
{% endpullquote %}

<!-- more -->

## The ugly

You've installed your **nginx** server, it proxies some well-coded **Rails** applications served by the nice-and-fast **thin** app server. All good.

But you know, these are verbose people. They like to talk. A lot. And as obedient software, they write every character of their chatter. So this is where your leak is coming from.

As a matter of fact, if you don't tell somebody to clean their mess, nobody won't... and you're server will soon die of low-disk-space agony.

## The bad

You will have to tell `logrotate` to do some work for these guys to keep their chatter under control.

`logrotate` is the cowboy which will rule the land ;) Tell it which log has to be rotated and it will automatically does the job for you. So now, get into this configuration stage...

The `logrotate` configuration file should be `/etc/logrotate.conf`.

You can add your instructions for log rotation by adding the following lines in this file, or you can create a new file under `/etc/logrotate.d` if you think this is a more organized way (I think). This latter option mimics the way packages configure `logrotate`. You will find several examples in this directory, so you may just look at them too.

```
/path/to/your/rails/applicaton/log/*.log {
  daily
  missingok
  rotate 7
  compress
  delaycompress
  notifempty
  copytruncate
}
```

If you want to force `logrotate` to run right now instead of waiting until tomorrow to see if your configuration is OK, you can run this command:

    /usr/sbin/logrotate -f /etc/logrotate.conf
    
Else you can just wait a little.

## The good

If you got through all this useless blabber about bad guys chatting and cowboys, you'll be happy to know that some serious~~ly boring~~ (no no, just kidding, I don't even know him) guy has written a far more efficient tutorial on the subject, and you can find it [here](http://www.nullislove.com/2007/09/10/rotating-rails-log-files/).

This guide will help you understand how `logrotate` works and the purpose of each line in the code above.