---
title: 'Play with Swift & Swift 1.2'
permalink: play-with-swift
id: 17
updated: '2015-02-11 10:40:22'
date: 2015-01-16 11:43:58
tags:
---

* 在Swift里面，`private`前缀对同一个文件内的类无效。也就是说，下面这种情况，就算是private也可以访问。
```
class A {
    private var key = "KEY"
    
    private func secret() {
        println("Nobody will know my secret")
    }
}

class B {
    private func hacker() {
        let a = A()
        a.secret()
        println("I'm a hacker")
        println(a.key)
    }
}

let b = B()
b.hacker() 
// Nobody will know my secret
// I'm a hacker
// KEY
```

* Swift 1.2中, `??`的运算顺序被提升了，以前，类似这种情况`let a = b as? [Float] ?? [1.0, 2.0]`，也就是说b有可能是一个Float的Array，如果不是的话，`b as? [Float]`就会是`nil`，这种情况下我们用`[1.0,2.0]`来初始化a。改版后，必须要给前面那个optional值加括号，变成`let a = (b as? [Float]) ?? [1.0, 2.0]`

* Swift 1.2中还有一个变化是Global的Computed Property不能用了，这点有点奇怪。在Playground里面没什么问题，但是在项目里面如果有类似下面的Global Computed Property的话，编译就会出`Segment Error`。
```
// 获取当前时间的字符串形式
var Timestamp: String {
    let dateFormatter = NSDateFormatter()
    dateFormatter.dateStyle = .ShortStyle
    return dateFormatter.stringFromDate(NSDate())
}
```