---
title: 'Animated Play & Pause Button written in Swift'
permalink: animated-play-pause-button-written-in-swift
id: 2
updated: '2014-09-06 22:36:20'
date: 2014-09-06 22:26:20
tags:
---

[GitHub](https://github.com/KelvinJin/PlayPauseButton)

**How to use**
```swift
// Initial the button with a particular frame
self.button = PlayPauseButton(frame: CGRectMake(0, 0, 50, 50))

// Add actions
self.button.addTarget(self, action: "toggle:", forControlEvents:.TouchUpInside)

// In the toggle function:
func toggle(sender: AnyObject!) {
	self.button.showsMenu = !self.button.showsMenu
}
```

**Customize**
```swift
// Set the stroke color
self.button.strokeColor = UIColor.blueColor().CGColor

// Set the duration time
self.button.duration = 0.5

// Set the fill color
self.button.fillColor = UIColor.redColor().CGColor

// Set the line width
self.button.lineWidth = 2.5
```

![](/content/images/2014/Sep/play_pause_button.gif)
