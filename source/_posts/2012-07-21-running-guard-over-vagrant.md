---
layout: post
title: "Running guard over Vagrant"
date: 2012-07-21 18:05
comments: true
published: true
categories: [virtualization, vagrant, ruby, guard, development]

---

To keep it simple, you can't... do it properly. File events are not correctly triggered by VirtualBox on the guest when the files are updated from the host (see the related Github [issue](https://github.com/guard/listen/issues/53)). So **guard cannot detect the events issued by the host**... unless you run it with the polling option:

```
vagrant -p
```

This polling option works correctly on the latest versions of VirtualBox (there was an [issue](https://github.com/guard/guard/issues/269) before 4.1.12), but it remains a bad option because doing so my **system's CPU instantly goes to a constant 100% charge**.

To **improve the situation**, you can still **reduce the latency** so that the polling is not done so frequently. This naturally adds some delay before the triggering of the events, but it will keep your system cool.

```
guard -p -l 10
```