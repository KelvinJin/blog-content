---
title: Advanced Swift - Part 1
permalink: advanced-swift
id: 3
updated: '2015-01-06 12:00:56'
date: 2014-12-17 09:47:46
tags:
---

######Access Control
Swift supports access control. There levels are provided, `private`, `internal`(default) and `public`. `set` can be restricted in the current file if `private(set)` is used. Copied from [Swift blog](https://developer.apple.com/swift/blog/):

```
public class ListItem {

	// Public properties.
	public var text: String
	public var isComplete: Bool

	// Readable throughout the module, but only writeable from within this file.
	private(set) var UUID: NSUUID

	public init(text: String, completed: Bool, UUID: NSUUID) {
		self.text = text
		self.isComplete = completed
		self.UUID = UUID
	}

	// Usable within the framework target, but not by other targets.
	func refreshIdentity() {
		self.UUID = NSUUID()
	}

	public override func isEqual(object: AnyObject?) -> Bool {
		if let item = object as? ListItem {
			return self.UUID == item.UUID
		}
		return false
	}
}
```

######Pointers in Swift

`let` array is equivalent to `UnsafePointer<T>`, `&var` is equivalent to `UnsafeMutablePointer<T>`(Note the `&`).

```
let a: [Float] = [1, 2, 3, 4]
let b: [Float] = [-1, -2, -3, -4]
var c: Float = 0.0

func dotMultiplication(a: [Float], b: [Float], inout c: Float) {
    for i in 0..<a.count {
        c += a[i] * b[i]
    }
}

func pointerDotMultiplication(a: UnsafePointer<Float>, b: UnsafePointer<Float>, c: UnsafeMutablePointer<Float>, size: Int) {
    for i in 0..<size {
        c.memory += a.advancedBy(i).memory *  b.advancedBy(i).memory
    }
}

// They have the same effect.
dotMultiplication(a, b, &c)
pointerDotMultiplication(a, b, &c, 4)

// Note the difference between the definition of the functions and the similarity when we pass the arguments.
```

As we can tell from the name, directly operating the pointers in Swift is not a good practice. It's highly recommanded that we can rewrite our codes using `inout` combined with `let`s and `var`s to better fit the style of Swift.

######Lazy Globel Variable

> The lazy initializer for a global variable (also for static members of structs and enums) is run the first time that global is accessed, and is launched as `dispatch_once` to make sure that the initialization is atomic. This enables a cool way to use `dispatch_once` in your code: just declare a global variable with an initializer and mark it private.

######Type Casting
We can see in Swift, you can cast a `Int` variable to `Float` by doing `Float(1)`. This is because `Float` has an initializer with the following style:
```
func Float(_ i: Int) {
	// Do the actual conversion.
}
```
The underscore `_` here allows the syntax `Float(1)` to use instead of `Float(i: 1)`.

######Rewrite
```
prefix func +(lhs: Int) -> Int {
    return 1 - lhs
}

let e = +100
```

