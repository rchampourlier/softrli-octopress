---
layout: post
title: "Setup development environment with Vagrant"
date: 2012-08-07 21:00
comments: true
published: true
categories: [development, environment, vagrant, virtualization]

---

### Install Vagrant

First, we want to install Vagrant on our development machine. Go to [the Vagrant website](http://vagrantup.com/) and download the latest binaries for your OS. If there is no binary package available, you can install it through Rubygems using `gem install vagrant`, but Vagrant's guide tells us binaries are the best choice.

### Download your first Vagrant virtual machine

To build its development environments, Vagrant relies on Virtual Box virtual machines with a barebones OS installation. We will work with the common Ubuntu 10.04.3 LTS, also known as Lucid Lynx. So we have to download the corresponding Vagrant box, which is done by:

```
$ vagrant box add lucid32 http://files.vagrantup.com/lucid32.box
```

This box is a fully functional Ubuntu Lucid machine with 512MB of RAM. As soon as the command ends, we're ready to go on.

*Note that if you are already using Vagrant and added this box before, you should already be ready for the next part.*

### Configuring the project to use the Vagrant box

Go to your project's directory, and run this to init Vagrant with the project:

```
$ vagrant init lucid32
```
This will create the Vagrant's configuration file, `Vagrantfile`. The generated files contains already lots of commented commands to setup the configuration. For now, we replace this all by this:

```
Vagrant::Config.run do |config|
  config.vm.box = "lucid32"
end
```

### Run the Vagrant box

Run `vagrant up` to setup and start the virtual machine. It can't do much for now, but it starts! If everything is good, you should see this finish message: 

```
[default] VM booted and ready for use!
[default] Configuring and enabling network interfaces...
[default] Mounting shared folders...
[default] -- v-root: /vagrant
```

However, if the process seems to stop on `[default] Waiting for VM to boot. This can take a few minutes.` you may have the same issue I had with Vagrant not achieving to connect to the virtual machine with the default network configuration (NAT). There is [an issue on Github](https://github.com/mitchellh/vagrant/issues/455) related to this problem. The cause of this issue is currently unclear, but on my system adding this to the `Vagrantfile` has made it work without any hickup for several months:

```
# Tries to fix issue #455
  config.vm.customize ["modifyvm", :id, "--rtcuseutc", "on"]
  config.ssh.max_tries = 10
```

### Preparing the environment

From now, the box you installed is not provisionned with any server software. To do so, the best way is to have built a set of Chef or Puppet recipes (or cookbook) that you will specify into `Vagrantfile`. Doing so, `vagrant up` will provision the required environment in one single step. We won't cover this subject in here, since it focuses on setting up Vagrant itself. If you want some help to setup Chef for example, I wrote several articles on the subject, check [the vagrant category](http://www.softr.li/blog/categories/vagrant/).


### References

[http://docs.puppetlabs.com/puppet_core_types_cheatsheet.pdf](http://docs.puppetlabs.com/puppet_core_types_cheatsheet.pdf)
[https://github.com/example42/puppet-modules](https://github.com/example42/puppet-modules)
[http://www.example42.com/](http://www.example42.com/)
