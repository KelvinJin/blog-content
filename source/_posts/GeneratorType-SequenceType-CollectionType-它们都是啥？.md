---
title: 'GeneratorType, SequenceType, CollectionType 它们都是啥？'
permalink: generatortype-sequencetype
id: 39
updated: '2016-02-26 23:23:52'
date: 2016-02-26 22:04:09
tags:
---


```swift
//: Playground - noun: a place where people can play

import AppKit


```

 今天给大家介绍几种我们常见的Swift协议。平时用的时候我们并没有太关注它们的细节，
 但是了解多一点肯定会对以后使用有所帮助。

####GeneratorType
 
 这三种协议里面最基础的就是`GeneratorType`了。看看它的定义就知道为什么这么说：

```swift


public protocol GeneratorType {
    associatedtype Element
    @warn_unused_result
    public mutating func next() -> Self.Element?
}


```

 `GeneratorType`的定义非常简单，一个相关类型`associatedType`代表我们要“生成”的元素类型，
 一个方法用来返回下一个元素。你可能会很好奇，为什么它只有`next`函数，而没有`last`函数呢？
 因为`GeneratorType`协议并不保证“生成”的元素不被销毁，就像是时间一样，过了今天，今天就没了：（。
 下面是一个简单的实现示例。

```swift

struct MyGenerator: GeneratorType {
    typealias Element = Int
    
    private var counter = 0
    
    mutating func next() -> MyGenerator.Element? {
        return counter++
    }
}


```

那你可能会问了，仅仅实现一个`Generator`好像并没有什么卵用啊。确实是这样。这就是为啥我们要来看看`SquenceType`.

####SquenceType
 
如果你说这玩意有个毛用啊？那你就too young了，你平时用的`for...in`全靠它。换句话说，如果你写`for x in A`，那么`A`一定要实`SequenceType`这个`protocol`。所以如果我们能想个办法把上面的`MyGenerator`变成一个实现了`SequenceType`的类型，那它就可以用的上了。看看下面的例子：

```swift

for x in GeneratorSequence(MyGenerator()) {
    guard x < 100 else { break }
    print(x)
}


```

 看，我们一下子就把一个没什么鸟用的`GeneratorType`变成了一个自然数打印机！
 我们靠的就是这个`GeneratorSequence`结构体，一个非常简单的`SequenceType`实现。
 别看这个转换就一行代码，一旦转换成`SequenceType`类型，那些我们平时经常使用的函数方法就
 瞬间可用了。比如`map`, `first`, `contains`等等。而且完全就是免费获得，一行代码不用写。
 除了用`GeneratorSequence`这个结构体以外，也可以用anyGenerator这个全局函数来转换成`AnyGenerator`结构。

```swift

anyGenerator(MyGenerator()).contains(1000)


```

####CollectionType

 好了，既然`SequenceType`如此牛逼，为啥还要`CollectionType`呢？
 这就要回到我们刚才说到的一个问题，`Generator`并不要求在遍历元素后保留它，
 也就是说，一旦一个元素被“生成”出来了，它就不再属于这个`Generator`了。
 而`SequenceType`也具有同样的特性，因为它仅仅是对`GeneratorType`的一个封装，扩展。
 这不行啊，有时候我们也需要数组这种能够把东西存起来，我们可以想添加就添加，想删除就删除。
 比如我在数组里放一堆片，我想要今天看完一部片后留着以后继续看，不能说看了这部片以后就不能看了，
 那你这不是让我撸断肠？？这时候`CollectionType`就有用了，我们可以放心的把片存起来：）。
 下面这个简单的例子解释了这种只能看一次的尴尬。。。

```swift

let onceSequence = anyGenerator(MyGenerator())

var counter = 0
for today in onceSequence {
    if today == 100 {
        print("遍历\(counter)次") // 遍历100次
        break
    }
    counter++
}

counter = 0
for tomorrow in onceSequence {
    if tomorrow == 200 {
        print("遍历\(counter)次") // 遍历99次
        break
    }
    counter++
}


```

 今天到这里咯，下次见！

PS: 这整个博客都是用Xcode的Playground写完后自动导出成的Markdown。使用了我写的一个Xcode插件，去[Github](https://github.com/KelvinJin/PlaygroundExporter)下载吧!