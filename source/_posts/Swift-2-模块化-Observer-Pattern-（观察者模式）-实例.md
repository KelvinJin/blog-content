title: Swift 2 模块化 - Observer Pattern （观察者模式） 实例
permalink: swift-2-mo-kuai-hua-observer-pattern-shi-li
id: 32
updated: '2015-08-21 21:55:20'
tags:
  - Swift
  - Protocol
categories:
  - Programming
date: 2015-08-18 05:50:00
---
由于我目前开发的软件还没有升级到Swift 2，所以一直没有机会研究一下Swift 2。最近看了WWDC 2015的视频[Introducing GameplayKit](https://developer.apple.com/videos/wwdc/2015/?id=608)受到了一些启发。

<!--- more --->

无论是在Objective C还是在Swift里面，尽管我们可以让子类Conform多个协议，但是只能最多继承自一个父类。也就是说下面这种代码是无效的：

```Swift
class A {}
class B {}
class C: A, B {} // error: multiple inheritance from classes 'A' and 'B'
```

除此之外，如果希望在不同的Class间分享代码，那么我们就不得不让这些类都继承自同一个父类。比如：

```Swift
class Movable {}
class Car: Movable {}
class Boat: Movable {}
```

但是这样的问题是Car和Boat同时也都是Sellable（可以销售的），那么我们就尴尬了。你可以会想，那不如让Movable继承自Sellable或者让Sellable继承自Movable，然后再让Car, Boat继承自它们，就像下面这样:

```Swift
class Movable {}
class Sellable: Movable {}
class Car: Sellable {}
class Boat: Sellable {}
```

但是问题是这个和我们的常识相悖，并不是所有可以移动的东西都是可以拿来销售的（有一种很惊悚的感觉。。），也并不是所有商品都是可以移动的。当然也许你会想，那么我们新建一个中间类叫MovableGoods（可移动商品），然后让Car和Boat继承自它。像这样：

```Swift
class Movable {}
class Sellable {}
class MovableGoods: Movable, Sellable {}
class Car: MovableGoods {}
class Boat: MovableGoods {}
```

好吧，那如果某天你发现，好像我还需要添加一个类Ridable（可以骑的）。。。估计下一步你会创建一个中间类叫MovableRidableGoods， 而且为了区分 **可移动可销售的**， **可移动可骑的** 和 **可销售可骑的**，你估计还要另外创建三个类。。。不好意思，我不是故意为难你，不过在Objective C或者Swift 1.X里面，我们似乎没有选择。

###传统的观察者模式 Observer Pattern

大家应该对这个模式相当熟悉，几乎任何设计模式的书都会提到。这个模式的基本架构是有一个被观察者和好多观察者，一旦被观察者的状态更新了，我们就应该通知所有的观察者。我知道在iOS中我们可以使用KVO或者Notification来实现类似的功能，但是在这里我不会去评价KVO和Notification有什么不同，或者KVO和Observer Pattern哪个好。这里我先给出传统的Observer Pattern:

以下是从Journaldev借用过来的Java版本的Observer和Subject接口定义（去掉了一些可有可无的函数）。

```Java
package com.journaldev.design.observer;
 
public interface Subject {
 
    //methods to register and unregister observers
    public void register(Observer obj);
    public void unregister(Observer obj);
     
    //method to notify observers of change
    public void notifyObservers();
}
```

```Java
package com.journaldev.design.observer;
 
public interface Observer {
     
    //method to update the observer, used by subject
    public void update();
}
```

Subject也就是被观察者，Observer就是观察者。Subject 里面最重要的两个函数是`register`和`notifyObservers`。`register`将传入的一个观察者放到自己维护的一个观察者序列里面，然后每当它的状态改变，就可以调用`notifyObservers`来通知所有的观察者。而`notifyObservers`主要就是遍历观察者序列里面所有的观察者，然后调用他们的update函数。

###Ruby 里面的观察者模式

Ruby里面有个很重要的类型是module。module使得我们可以动态的给类添加模块。如果我们想要给一个class添加被观察者的功能，只需要像这样：

```Ruby
require "observer"

class Ticker          ### Periodically fetch a stock price.
  include Observable

  ...
end
```

然后你就可以调用`ticker.add_observer`来添加新的观察者或者`ticker.notify_observers`来通知所有观察者。可以看出来，这种模块化非常的有意义，因为任何对象都可以成为被观察者。然而我们在Swift 1.2或者Objective C里面除了使用继承来共享这种功能以外并没有太多选择，而单一继承的限制又让我们无所适从。

##Swift 2 Observer 模块

好消息是，在Swift 2里面，我们可以扩展协议（extension protocol），这使得我们有机会实现这种模块化。看看下面这种实现：

```swift
//: Playground

import UIKit
import Foundation
import ObjectiveC

protocol Observeable {}

protocol Observer {
    func update(info: Any?)
}

class AssociatedObserverWrapper {
    static var keys = "nsh_Observers"
    var observers: [Observer] = []
}

extension Observeable where Self: AnyObject {
    
    var wrapper: AssociatedObserverWrapper {
        get {
            
            if let _wrapper = objc_getAssociatedObject(self, &AssociatedObserverWrapper.keys) as? AssociatedObserverWrapper {
                return _wrapper
            }
            
            let newWrapper = AssociatedObserverWrapper()
            objc_setAssociatedObject(self, &AssociatedObserverWrapper.keys, newWrapper, objc_AssociationPolicy.OBJC_ASSOCIATION_RETAIN_NONATOMIC)
            
            return newWrapper
        }
        set {
            objc_setAssociatedObject(self, &AssociatedObserverWrapper.keys, newValue, objc_AssociationPolicy.OBJC_ASSOCIATION_RETAIN_NONATOMIC)
        }
    }
    
    func addObserver<T: Observer>(newObserver: T) {
        wrapper.observers.append(newObserver)
    }
    
    func notifyObservers(info: Any? = nil) {
        for observer in wrapper.observers {
            observer.update(info)
        }
    }
}

class Poster: AnyObject, Observeable {
    var name: String
    init(name: String) {
        self.name = name
    }
    
    func postMessage(msg: String) {
        print("Poster is posting: \(msg)")
        notifyObservers(msg)
    }
}

class Subscriber: AnyObject, Observer {
    var name: String
    init(name: String) {
        self.name = name
    }
    
    func update(info: Any? = nil) {
        print("Subscriber \(name) received: \(info)")
    }
}

let subscriberA = Subscriber(name: "A")
let subscriberB = Subscriber(name: "B")
let subscriberC = Subscriber(name: "C")
let posterA = Poster(name: "Teacher")
posterA.addObserver(subscriberA)
posterA.addObserver(subscriberB)
posterA.addObserver(subscriberC)

posterA.postMessage("I release a new assignment!!!")

//Poster is posting: I release a new assignment!!!
//Subscriber received: Optional("I release a new assignment!!!")
//Subscriber received: Optional("I release a new assignment!!!")
//Subscriber received: Optional("I release a new assignment!!!")
```

也就是说，我们实现了模块化Observer。如果你新定义了一个类，并且想要它成为一个被观察者，只需要让他Conform Observerable协议就行。其实说到底，这个`extension protocol`某种意义上使得我们可以实现多重继承。