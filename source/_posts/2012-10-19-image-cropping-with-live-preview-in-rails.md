---
layout: post
title: "Cropping with live preview in Rails, using Jcrop"
date: 2012-10-19 11:50
comments: true
published: true
categories: [web, development, rubyonrails, javascript]

---

Well, thanks to the [Jcrop Railscast](http://railscasts.com/episodes/182-cropping-images), I had setup **avatar upload and cropping features** on [our project](http://www.avdice.com).

With the major changes coming in our next release, this little feature **wasn't working anymore**, and I couldn't figure why my preview was working like a mess...

I looked into the code, which matches the [Jcrop's original example](http://deepliquid.com/projects/Jcrop/demos.php?demo=thumbnail), but I couldn't figure what was wrong! Wow, but that's a pretty simple thing, no? Just scaling and offsetting a picture, shouldn't take long to understand?!

Well, the code wasn't looking good enough. I'm undertaking [edX SAAS course](https://www.edx.org/courses/BerkeleyX/CS169.1x/2012_Fall/about) so I want my code to be _beautiful_. So, let's rewrite it!

### The context

* I want a **simple cropping feature**, using Paperclip just like the one setup in the Railscast, and the **live preview**.
* I just need to crop to a **square picture**.
* I want it to work!

I won't rewrite the whole tutorial, since the Railscast is perfect to setup your model with Paperclip, add the cropper module, but the Javascript with preview was a wreck, and that's what I rewrote.

### The Javascript

```
:javascript
  
  var previewSize = 150;
  var originalSize = {
    width: #{@user.avatar_geometry(:original).width},
    height: #{@user.avatar_geometry(:original).height}
  };

  function updatePreview(coords) {
    var ratio = previewSize / coords.w;
    
    $('#preview').css({
      width: Math.round(originalSize.width * ratio) + 'px',  
      height: Math.round(originalSize.height * ratio) + 'px',  
      marginLeft: '-' + Math.round(coords.x * ratio) + 'px',  
      marginTop: '-' + Math.round(coords.y * ratio) + 'px'  
    });

    $('#profile_crop_x').val(Math.floor(coords.x));  
    $('#profile_crop_y').val(Math.floor(coords.y));  
    $('#profile_crop_w').val(Math.floor(coords.w));  
    $('#profile_crop_h').val(Math.floor(coords.h));    
  };

  $(function() {
    $('#cropbox').Jcrop({
      onChange: updatePreview,  
      onSelect: updatePreview,  
      setSelect: [0, 0, originalSize.width * 0.9, originalSize.height * 0.9],  
      aspectRatio: 1
    });
  });
```

I think this version makes more sense, and in my case it works perfectly. I used some drawings to help me rewriting this, so you may find it helpful, in particular if you want something to start with if you don't just want a square target size!

![](https://s3-eu-west-1.amazonaws.com/softrli/assets/2012-10-19-image-cropping-with-live-preview-in-rails-asset-1.png)