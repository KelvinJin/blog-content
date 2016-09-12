---
title: Swift Note
permalink: untitled-11
id: 20
updated: '2015-03-13 19:45:28'
date: 2015-02-09 14:18:18
tags:
---

* NSURL(string: ) 和 NSURL(fileURLWithPath: ) 一定要区分开。一般情况下，一个NSURL的结构都是(prefix)://(path)，如果拿到的仅仅是path的话，一定要用后者初始化URL。

* Swift 里面的lazy modifier就不用多说了，另外一个就是final，这个modifier在好多语言里面都有，只能放在class或者class member的前面如果你确定这个类或者类成员不会被override就行，这种情况下compiler会进行最大程度的优化（release mode下）。如果不使用额外的modifier，默认会使用swift的static dispatcher来找对应的method。如果使用dynamic，则会用objective-c的dynamic dispatcher也就是说用发消息的方法。后者会支持很多runtime的特性。

* Swift 在release模式下默认进行fastest optimization，造成很多莫名其妙的bug。这点很容易被开发者忽略。

* Swift 在调用方法的时候用的static dispatcher所以其实没有SEL这个参数，这样的好处是可以多一个parameter放在register里面而不是stack里。加快了读写速度。

* 如果你需要给变量一个别的名字。。。
var enabled: Bool {
@objc(isEnabled) get {
    /* ... */
}
}

* Swift 的property只要是let，不管是不是optional的都必须初始化。
```
class Person {
	let talent: Int? // 要么直接在这里初始化 = 1
    init(talent: Int? = nil) {
    	self.talent = talent // 要么在这里
	}
}
```
当然如果是var的话，optional的就不是强制初始化了。


3月13更新：

Xcode 6.3 Beta 3里面增加了一个flatMap函数用来对二维或多维数组做处理。比如说你希望在[[1, 2], [3, 4], [1, 5, 6]]这个数组里面找到 1这个元素所在的全部位置，那么你可以这么做：
```Swift

```