---
layout: post
title: "XCode failing when building for archiving because of a missing library link"
date: 2012-06-11 11:07
comments: true
categories: [development, xcode, ios]

---

### The issue

You may be using opensource libraries within your iOS project. (_If you don't, you should, it's a great time-saver. Check [here](http://pinterest.com/rchampourlier/best-ios-controls/) for some starters._)

As you may know, XCode may sometimes become really painful when dealing with the **compilation and linking of external libraries**. It may just work fine when compiling for development, but when you want it compiled for distribution, XCode may just not agree anymore.

I just faced such an issue this morning. This time, I met the `ld: library not found for...` error: 

{% img /images/2012-06-11-screenshot-1.png %}

<!-- more -->

Googling a little, I found these two StackOverflow answers, but they could not help me. You can give a try, this may still contain the answer to your own issue.

### The solution

* [Compile, Build or Archive problems with Xcode 4 (and dependancies)](http://stackoverflow.com/questions/5584317/compile-build-or-archive-problems-with-xcode-4-and-dependancies)
* [Missing Library link error when doing Product > Build For Archiving in Xcode 4](http://stackoverflow.com/questions/6004919/missing-library-link-error-when-doing-product-build-for-archiving-in-xcode-4)

I checked where the generated could be, and looking at the logs of the build process, I could find the directory's path: `/Users/me/Library/Developer/Xcode/DerivedData/.../`

Checking the content of this directory, I discovered that my issue was that the library was indeed not build for the `Distribution-iphoneos` configuration:

{% img /images/2012-06-11-screenshot-2.png %}

Correcting the issue was as simple as going to the library's project settings and adding the `Distribution` configuration:

{% img /images/2012-06-11-screenshot-3.png %}

### The conclusion

Be sure to check that the libraries you include within your XCode project have all the necessary configurations setup!