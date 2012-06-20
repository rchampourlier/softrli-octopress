---
layout: post
title: "Which HTML5 Canvas Javascript library should I use?"
date: 2012-06-20 12:35
comments: true
categories: [development, webdev, html5, canvas]

---

I'm looking for an HTML5 Canvas library which I could use to build an interactive and animated UI control. So my requirements are essentially:

* well **documented**, **supported** and **maintained** library, because I want to be able to reuse it in later projects,
* easing the process of creating **complex graphical objects** (my control is going to be quite complex, a lot more than a simple button), if possible supporting groups and layers,
* helping me with the handling of **user interactions**,
* supporting **touch devices**,
* providing tools for **animating** the graphical content.

I've searched for the appropriate library, so I will share with you the results of my search. After a list of most of the available libraries, I investigated the following shortlist of libraries: **EaselJS**, **fabric.js**, **Paper.js**, **processing.js** and **Kinetic.js**. I hope this can help you with your own choice!

<!-- more -->

### Found libraries

Here is the list of what seems to be the main still-maintained Canvas libraries.

* [CAAT](https://github.com/hyperandroid/CAAT)
* [EaselJS](http://www.createjs.com/#!/EaselJS)
* [Fabric.js](http://fabricjs.com/)
* [Gury](http://guryjs.org/)
* [JcanvaScript](http://jcscript.com/)
* [Kinetic.js](http://www.kineticjs.com/)
* [oCanvas](http://ocanvas.org/)
* [Paper.js](http://paperjs.org/)
* [processing.js](http://processingjs.org/)

Here are some other libraries I have not investigated, either because they weren't on Github or seemed abandoned:

* [cakejs](http://code.google.com/p/cakejs/)
* [Doodle-js](https://github.com/lamberta/doodle-js)
* [CanvasToolkit](https://canvastoolkit.codeplex.com/)
* [Mootools Canvas lib](http://mootools.net/forge/p/mootools_canvas_lib)

### The Github comparison

Library       | Watch | Fork | Issues
------------- | ----- | ---- | ------
CAAT          | 336   | 42   | 17
EaselJS       | 1,440 | 203  | 90    
fabric.js     | 1,059 | 101  | 38    
gury          | 348   | 19   | 9     
jCanvaScript  | 80    | 5    | 2     
Kinetic.js    | 267   | 41   | 7    
oCanvas       | 194   | 20   | 6 
Paper.js      | 1,706 | 111  | 30
processing.js | 1,276 | 206  | N/A

### The minified size comparison

Library       | Size (kb)
------------- | ---------
CAAT          | 210
EaselJS       | 44
fabric.js     | 133
gury          | 11
Kinetic.js    | 53
oCanvas       | 18

_No minified version was available in the Github repo for jCanvaScript, Paper.js and processing.js._

### The Stackoverflow comparison

Search text   | Tag           | # tagged questions | # search questions
------------- | ------------- | ------------------ | ------------------
CAAT          | N/A           | N/A                | 5
EaselJS       | easeljs       | 30                 | 79
fabric.js     | fabricjs      | 43                 | 78
jCanvaScript  | N/A           | N/A                | 6
Kinetic.js    | kineticjs     | 74                 | 30
oCanvas       | N/A           | N/A                | 19
Paper.js      | paperjs       | 9                  | 49
processing.js | processing.js | 117                | 289 

_The gury library could not be found on StackOverflow. I've used N/A when I couldn't find a matching tag._

### Review from documentation, tutorials and other resources

When choosing a framework, I value most the Github comparison. It provides a good overview of the development state of the library and the community using it. Since I'm generally not going to an expert level in a given field, I like being able to rely on the community.

But StackOverflow (SO) is really helpful, in particular when the comparison question has already been asked. Check this: [Current state of Javascript canvas libraries](http://stackoverflow.com/questions/8938969/current-state-of-javascript-canvas-libraries2012)

As a consequence, I will give a deeper review on documentation, tutorials and other resources for **EaselJS**, **fabric.js**, **Paper.js**, **processing.js** and the outsider, **Kinetic.js** to make a choice.

#### Summary

**EaselJS**, **fabric.js**, **Paper.js**, **processing.js** can be seen as the 4 leaders. They have definitely the biggest communities of users, are available on Github, well documented, lots of external references (questions on SO, forums), and a good thing, according the the previous SO question, they are unit-tested.

**Kinetic.js** is the outsider. Added more recently on Github, it's still quite dynamic but received a warm welcome from [kangax](http://stackoverflow.com/users/130652/kangax), the author of Fabric.js, in a SO comment.

These 4 libraries are all available on Github, and released under the permissive MIT license.

#### EaselJS

This library is **part of the CreateJS suite** which is a full-featured set of libraries to build advanced HTML5 interactive and animated graphics.

In particular, combined with the animation library (TweenJS), complex animations should be buildable. You also get the SoundJS library and the asset preloading library (PreloadJS) so you should have everything you need if you intend to build a game.

The website provides some nice demos whose source code is available within the Github repository.

The library also seems to work well with other libraries such as Box2d and TexturePacker.

**Support of touch devices is built in.**

#### fabric.js

Looking at the [home page](http://fabricjs.com/), it seems this library is more oriented on building **vector edition tools**. The main features are:

* create and manipulate (move, scale, rotate...) vector shapes and text objects,
* import/export from/to SVG.

It's summarized as *"an interactive object model on top of canvas element"*.

**If your goal is to build complex scenes, animate objects, it seems to me it's not the right choice.**

#### Paper.js

This library is a port from the Scriptographer library. A particularity is it's Paperscript language, which is basically an extended Javascript providing some mathematical operations on point and size objects. However it's still [not documented](http://paperjs.org/tutorials/getting-started/paperscript-interoperability/).

This library seems powerful in its capacity to build complex vector objects and managing mouse interactions. However, there is no mention of touch devices support, and the animation capability seems to be limited to the `onFrame()` method which is called 60 times per second and allows you to change the content of the canvas.

#### processing.js

The core goal of this library is to build **interactive visualizations**.

This library has a particulary history since it's a port of the famous Processing library. I say famous, not because I knew it myself, but because it's said to be famous among the multimedia and art community which uses it to build **interactive artistic creations**.

Looking at the [first code example](http://processingjs.org/learning/), it seems that this library intends to lower the learning curve for building animated and interactive graphics with the Canvas. It provides the tools to easily have a run loop and a `draw()` method you simply fill to build your visualization.

I think it thus provide a **low learning-curve**, perfect for **non-developers artists and creative people**, but is **not the best-suited tool for building object-oriented components**.

#### Kinetic.js

Kinetic.js is the outsider of this comparison because looking to its Github repo, it's far from being the most used one. However, a search on "canvas library" will return the associated HTML5 tutorials as first result, and it has a good deal of questions on StackOverflow.

The name is a good clue, but this library is advertised as **pretty fast** with lots of objects thanks to the use of multiple canvases.

It is provided with a **nice documentation with tutorials**, with an introduction on barebones HTML5 canvas use, detailed documentation on Kinetic.js and Three.js. It also provides nice tips, not specific to the library itself, to perform some actions with canvas.

### Result

According to this review, I think I could go with one of **EaselJS** or **Kinetic.js**. **Paper.js** was not far but there is no mention of touch device support, so I'm pretty sure this would not be complicated to integrate, but I prefer to have something built in the library.

Finally, I will go with the outsider **Kinetic.js** because:

* I feel good with the example code,
* the author provides an excellent [set of tutorials](http://www.html5canvastutorials.com/),
* the documentation and examples are cleara and easy to read,
* everything I need is included in the library (I don't feel like I may need to add another library such as **TweenJS** to solve a small problem while loading another large library I'm using at 20%).

_Feel free to share your comments, reviews, or missing libraries!_