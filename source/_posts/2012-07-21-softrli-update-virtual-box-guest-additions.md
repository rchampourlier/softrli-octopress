---
layout: post
title: "How to update VirtualBox Guest Additions on a Vagrant virtual machine"
date: 2012-07-21 18:00
comments: true
published: true
categories: [virtualization, vagrant]

---

#### How to

```
wget -c http://download.virtualbox.org/virtualbox/4.1.18/VBoxGuestAdditions_4.1.18.iso
sudo mount VBoxGuestAdditions_4.1.18.iso -o loop /mnt
sudo sh /mnt/VBoxLinuxAdditions.run --nox11
```

At this point, **you may encounter an error** saying you need to specify `KERN_dir`. I spent some time finding what was required, finally after running all the following package installations it worked!

```
sudo apt-get install linux-headers-generic
sudo apt-get install dkms gcc
sudo apt-get install linux-headers-2.6.32-38-generic
```

I assume the most important is `dkms` and the appropriate `linux-headers-...` package. **Check the log message** provided with the error, it may indicate which package version has to be installed.

**Do not worry for the `Installing the Window System drivers ...fail!`** error message, this is no issue we have no window system on a vagrant box.

#### References

* [till.klampaeckel.de/blog/archives/155-VirtualBox-Guest-Additions-and-vagrant.html](http://till.klampaeckel.de/blog/archives/155-VirtualBox-Guest-Additions-and-vagrant.html)
* [ubuntuforums.org/showthread.php?t=1684185](http://ubuntuforums.org/showthread.php?t=1684185)