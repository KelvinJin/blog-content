title: iOS 动态字体大小一行代码
date: 2016-03-22 07:11:00
tags:
---
一行代码搞定动态字体大小。

1. 是不是还在由于如何支持辅助应用里面的放大字体设置？[现成的库][1]太复杂不太容易集成到interface builder?
2. 项目都是Swift不想用Objective-C?
3. 每个UIViewController或者自定义UIView都要监听UIContentSizeCategoryDidChangeNotification 事件？

那就试试这个简单的Helper吧，只有一个文件，拖进项目既可！

<!--- more --->

这是你会得到的效果：

![Demo GIF][image-1]

check [here][2]

[1]:	https://github.com/splinesoft/SSDynamicText
[2]:	https://github.com/KelvinJin/DynamicFontSizeHelper

[image-1]:	https://raw.githubusercontent.com/KelvinJin/DynamicFontSizeHelper/master/DynamicFontSizeDemo.gif