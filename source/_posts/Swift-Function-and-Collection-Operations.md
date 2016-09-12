---
title: Swift Function and Collection Operations
permalink: swift-notes
id: 24
updated: '2014-12-09 16:11:02'
date: 2014-12-04 15:50:00
tags:
---

####Function

---

######Modifiable Parameter
You can define a funcation parameter as a local variable when you pass it. By this way, a new local variable will be created as a modifiable object which is initialized by the parameter.
```
func concatTimeStamp(originalString: String) -> String {
    originalString = originalString + "\(NSDate().timeIntervalSince1970)"
    return originalString
}
// Error: Cannot assign to 'let' value 'originalString'

func concatTimeStamp(var originalString: String) -> String {
    originalString = originalString + "\(NSDate().timeIntervalSince1970)"
    return originalString
}
// No error.
```

######Modifiable Original Object
Using the `var` way, you will get a copied variable, however, sometimes, you want to change the original object itself in the function without using a class property (like what we do in C or C++ using pointer)

```
func swap(inout a: Int, inout b: Int) {
    let temp = a
    a = b
    b = temp
}

var a = 1
var b = 2

swap(&a, &b)

a // 2
b // 1
```
Inout method will not support `let` argument.

####Order functions
Swift provides some handy functions to deal with collections, which is similar to Ruby.

######Map
This is similar to `map` in Ruby:
```
let intArray = [1, 2, 3, 4, 5]
let stringArray = intArray.map { String($0) }
stringArray // ["1", "2", "3", "4", "5"]
```

######Filter
This is similar to `select`(or `reject`) in Ruby:
```
let evenArray = intArray.filter { $0 % 2 == 0 }
evenArray // [2, 4]
```

######Reduce
This is similar to `inject` (or `reduce`) in Ruby:
```
let sumOfArray = intArray.reduce(0) { $0 + $1 }
sumOfArray // 15

let productOfArray = intArray.reduce(1) { $0 * $1 }
productOfArray // 120
```