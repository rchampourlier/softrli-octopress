---
layout: post
title: "Linking local node modules with npm"
date: 2012-04-16 10:10
comments: true
categories: [node, node.js, npm, package, module, link, install]

---
... instead of installing from npm repository.

## Link a module to a local repo

When you need a fresher version of a module that the one published to npm repository (e.g. if you have your own patch, if new commits haven't been released yet), you may want to clone the module locally and have npm use it instead of the published one.

To do this, you **link** your local package.

    git clone https://github.com/repo/node-module.git
    cd node-module
    npm link
    
_You may need to `sudo` to perform the `link` command (this is because the link gets installed system-wide, in `/usr/local/lib/node_modules` on my MacOS X system)._

**Check the log of this command to see on which path npm references the linked module. In my case, the module was named as `node-module`, but npm was using `module` for the reference. In this case, be sure to link in your app (next section) using `module` and not `node-module`.**

## Link this module in your node application

You will now tell npm to use this linked version of the module for your application:

    cd your_application
    npm link module