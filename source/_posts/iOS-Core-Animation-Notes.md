---
title: iOS Core Animation Notes
permalink: ios-core-animation-notes
id: 23
updated: '2014-09-03 03:34:20'
date: 2014-09-02 23:51:07
tags:
---

####Layer
Layer 是一个图层，可以有背景色。
可以往Layer里面添加Content，可以是下列几种：


* Bitmap图
* 用delegate 来指定内容
* 用Quartz 来画

CALayer 有一个成员函数是 `presentationLayer()`, 它的作用是：

> Returns a copy of the presentation layer object that represents the state of the layer as it currently appears onscreen.

在 Swift 里面会返回一个非 optional 的AnyObject.

**Discussion**

The layer object returned by this method provides a close approximation of the layer that is currently being displayed onscreen. While an animation is in progress, you can retrieve this object and use it to get the current values for those animations.

The _sublayers_, _mask_, and _superlayer_ properties of the returned layer return the corresponding objects from the presentation tree (not the model tree). This pattern also applies to any read-only layer methods. For example, the hitTest: method of the returned object queries the layer objects in the presentation tree.

**Example on how to use Core Animation**
<script src="https://gist.github.com/arol/5567624.js"></script>