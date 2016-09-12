---
title: '[weak self] & [unowned self]'
permalink: weak-self-unowned-self
id: 30
updated: '2015-02-20 16:42:03'
tags:
---

##### Reference cycle for the classes

-----

In Swift, there are two ways you can prevent the strong reference cycle between classes. First, you can use `weak` to mark a property, indicating that this property might be `nil` in its lifetime. From apple's documentation: 
> Therefore, ARC automatically sets a weak reference to `nil` when the instance that it refers to is deallocated.

read more [here](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html#//apple_ref/doc/uid/TP40014097-CH20-ID51)

> A strong reference cycle can also occur if you assign a closure to a property of a class instance, and the body of that closure captures the instance.

----

> This strong reference cycle occurs because closures, like classes, are reference types.

----

> Define a capture in a closure as an unowned reference when the closure and the instance it captures will always refer to each other, and will always be deallocated at the same time.

----

> Conversely, define a capture as a weak reference when the captured reference may become nil at some point in the future.