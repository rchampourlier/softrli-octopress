---
layout: post
title: "Debugging TabBarKit, a barebones kit for custom tab bar on iOS"
date: 2012-05-10 12:36
comments: true
categories: [ios, cocoa, objective-c, custom ui, ui]

---

For our [Cultimots](http://itunes.apple.com/fr/app/cultimots-un-jeu-culture-vocabulaire/id483710651?l=fr&ls=1&mt=8) application, I wanted to customize the application's tab bar. I looked a little on Google, [cocoacontrols.com](http://cocoacontrols.com/), Github, but I didn't want to use a full-featured solution which may not remain configurable for what I add in mind.

I finally went for **TabBarKit**, which is quite barebones, **reproducing the basic features of `UITabBar`** while letting me **play with the code to get my own thing**.

On this way I encountered several time-consuming issues due to the non-existent documentation of TabBarKit and some bugs in the shared code. Since I already customized the code to fit my own project **I won't share the updated source code**, I will however share the issues I met and the solutions I found. Hope this will make your path faster than mine!

<!--more -->

### Confusion between tab bar style and tab bar item selection style

There is a confusion between the tab bar's style, which is a `TBKTabBarStyle`, and the tab bar item selection's style, which is a `TBKTabBarItemSelectionStyle`.

This confusion is explicit in these lines:

```objc TabBarKit/Classes/TBKTabBarController.m:119
TBKTabBarItem *tabItem = [[[TBKTabBarItem alloc] initWithImageName:controller.tabImageName style:self.tabBarStyle tag:tagIndex title:controller.title] autorelease];
```

```objc TabBarKit/Classes/TBKTabBarItem.m:170
-(id) initWithImageName:(NSString *)anImageName style:(TBKTabBarItemSelectionStyle)aStyle tag:(NSInteger)aTag title:(NSString *)aTitle {
```

The tab bar item `init` method expects `aStyle` to be a `TBKTabBarItemSelectionStyle` but the controller provides its own style, which is a `TBKTabBarStyle`. This is OK in the source code because in fact the both style are `enum`s with matching integer values. If you add yours, just be sure to clean this mess.

## Enabling titles

In the source code from the Github repo, titles are disabled. To restore them, you will have to uncomment some lines from `TabBarKit/Classes/TBKTabBarItem.m`:

```objc TabBarKit/Classes/TBKTabBarItem.m:178-188`
if (self.controllerTitle && self.selectionStyle == TBKTabBarItemDefaultSelectionStyle) {
		/*
		self.displayTitle = YES;
		self.titleLabel.font = [UIFont boldSystemFontOfSize:10.0];
		self.titleLabel.textAlignment = UITextAlignmentCenter;
		self.titleLabel.contentMode = UIViewContentModeLeft;
		self.imageEdgeInsets = UIEdgeInsetsMake(0, 22, 11, 0);
		self.titleEdgeInsets = UIEdgeInsetsMake(0, -35, 2, 0);
		[self setTitle:self.controllerTitle forState:(UIControlStateNormal | UIControlStateSelected)];
		*/
	}
```

There is also a bug on this line:

```objc TabBarKit/Classes/TBKTabBarItem.m:186
[self setTitle:self.controllerTitle forState:(UIControlStateNormal | UIControlStateSelected)];
```

If you set the title for both `UIControlStateNormal` and `UIControlStateSelected`, it won't show up for the tab bar items which are not selected, and their layout will get broken. Just replace the line by this:

```objc TabBarKit/Classes/TBKTabBarItem.m:186
[self setTitle:self.controllerTitle forState:(UIControlStateNormal)];
```

## Finishing touch

### Grey titles for the non-selected items

Update the lines 170 to 175 to have this in `TabBarKit/Classes/TBKTabBar.m`:

```objc TabBarKit/Classes/TBKTabBar.m:170-175
for (TBKTabBarItem *tab in self.items) {
	currentBounds.origin.x += self.tabMargin;
	tab.frame = currentBounds;
	currentBounds.origin.x += currentBounds.size.width;
	[self addSubview:tab];
	tab.titleLabel.textColor = [UIColor grayColor]; // added line
}
```

Update the `setSelected:` method in `TabBarKit/Classes/TBKTabBarItem.m:213-217`:

```objc 
else {
	if ([self.layer.sublayers containsObject:self.selectionLayer]) {
		[self.selectionLayer removeFromSuperlayer];
	}
   self.titleLabel.textColor = [UIColor grayColor]; // added line
}
```

**Your tab bar should already look great! The rest is customization, and it's up to you! Have fun!**