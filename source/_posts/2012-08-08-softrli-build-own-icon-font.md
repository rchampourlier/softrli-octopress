---
layout: post
title: "Build your very own icon font"
date: 2012-08-08 22:50
comments: true
published: true
categories: [web, css, icons, font, html]

---

I intend to give you **the fastest road to your own icon font**. How fast?

1. You find SVG files of the icons you want in your set. Except giving you some sources to find them (which I do), I'm afraid I can't save you this step.
2. Use the magic of [IcoMoon](http://keyamoon.com/icomoon/app), an HTML5 application brought to you by Keyamoon, to **package all these icons in a set of fonts, CSS and JS**. Once this is done, you're ready to use your brand new custom icon font!
3. That's true, no step 3!

<!-- more -->

### Introduction

There are plenty of reasons of using a custom icon font instead of sprites, if you're still not convinced, you can do a web search, but:

* it's **easier to build** with the tip I give you here,
* it's **vector**, so you can get your icons at **whatever-size-you-want**,
* it's **less data and requests** for your users (one font file for any size, any color, and it can be embedded in the CSS),
* many more...

There are **plenty of icon sets available** as fonts for free on the web, even apps allowing you to build custom sets from a selection of icons. However, you may have some icons on your own, and making your custom set is often the key to an homogeneous design. Here are some sources I've found:

<object width="560" height="420" id="pt-embed-4167394-658-object" type="application/x-shockwave-flash" data="http://cdn.pearltrees.com/s/embed/getApp"><param name="flashvars" value="lang=fr_FR&amp;embedId=pt-embed-4167394-658&amp;treeId=4167394&amp;pearlId=33352719&amp;treeTitle=icons&amp;site=www.pearltrees.com%2F" /><param name="movie" value="http://cdn.pearltrees.com/s/embed/getApp" /><param name="wmode" value="opaque" /><param name="allowscriptaccess" value="always" /><a href="http://www.pearltrees.com/rchampourlier/icons/id4167394" alt="icons" style="text-decoration:underline;"><span style="font-size:14pt;color:black;font-weight:bold">icons</span><span style="font-size:10pt;color:#999999;font-weight:normal"> et icons font/set composers / misc dans dev / Romain Champourlier (rchampourlier)</span></a></object>

**Now let's get started!**

### Get some SVG files for your icons

First you need a **good set of SVG files**. You can find them on the web (in the sources above for example), or build them yourself.

Other options may work well too, but to be sure your font looks good, you should ensure:

* all SVG files have the same size (I use 512x512pt),
* the path is merged,
* the icon is centered horizontally (I'm not quite sure what the best option for vertical alignment is, I centered and I'm fine).

_(To edit these files you can use Inkscape with is opensource.)_
	
### IcoMoon, the magical HTML5 app for your custom icon fonts needs

Well, just go [there](http://keyamoon.com/icomoon/app). The application is **easy to use, reliable, well designed**. It's HTML5 and you can even download a JSON file to save your config. You can thus build several sets of fonts, reload them later. You can even reload a generated font to edit it. This app is **awesome**!

Once you've uploaded your set of icons or chosen some from the available sets, you select the ones you want in your font and click the "Font" button.

{% img /images/2012-08-08-screenshot1.png %}

The last step before downloading the font is to set some options. I mostly went with the defaults, except for the "Reset Encoding". I'm using "Private Use Area", so that if the font can't be load the icons don't get replaced by random characters.

{% img /images/2012-08-08-screenshot2.png %}

Finally, I set a name for my font and I use the Base64 and CSS embedding option. As such, my font is already available in the generated CSS so it makes even 1 request less for the users' browsers!

{% img /images/2012-08-08-screenshot3.png %}

Now is the download time!

You're getting the fonts, the CSS file providing you with the styles to use your icons. The styles are automatically based on the name of your icon files. You also get a `JS` file to help Internet Explorer 6 & 7 with these icon fonts. Finally, an `index.html` file you can use to **browse your icons**, along with the CSS classes!

{% img /images/2012-08-08-screenshot4.png %}

So, wasn't it fast? Don't hesitate to share if you know a fastest way!