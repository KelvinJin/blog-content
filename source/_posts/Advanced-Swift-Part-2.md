---
title: Advanced Swift - Part 2
permalink: advanced-swift-part-2
id: 34
updated: '2015-01-06 12:00:24'
date: 2015-01-06 10:34:43
tags:
---

+ Build in identifiers `__FILE__`, `__FUNCTION__`, `__LINE__` and `__COLUMN__` can be used anywhere.

+ `StaticString` is "knowable during compile-time". No memory management stuff will be associated with this type of string overhead.

+ `UWord`, `Word` are two integer types which occupy one computer word. In 64-bit iOS system, they are the same as `UInt` and `Int` (typealias) seperately.

+ decimal, octal
```
let hexadecimalDouble = 0xC.3p0 // 12.1875, after decimal, the exponent will be -1, -2, -3..., so this one stands for (12*16^0 + 3*16^-1)
let exponentDouble = 1.21875e1 // 12.1875

0xFp2 means 15 × 22, or 60.0.
0xFp-2 means 15 × 2-2, or 3.75.

let octalInteger = 0o21 	// 17 in octal notation
let binaryInteger = 0b10001       // 17 in binary notation
```

+ In order to check whether a sequence contains a specific element, we can do:
```
let a = [1,2,4,56,7]
contains(a, { $0 > 50 }) // true
contains(a, 56)
```
It will return once it finds the element satisfying the condition without checking the rest.

+ `dropFirst` and `dropLast` can be used to get rid of the first or the last element of the array seperately.

+ When you loop through an array, if you want to get both the index and the value, instead of writing:
```
let ar = [1, 2, 3, 4, 5]
for i in 0..<ar.count {
	println("\(i) is \(ar[i])")
}
```
you can use `enumerate`:
```
let ar = [1, 2, 3, 4, 5]
for (ind, val) in enumerate(ar) {
    println("\(ind) is \(val)")
}
```

+ Compare two arrays `equal(ar, br, { $0 == 2 * $1 })`

+ `join` is used to join elements with the custom seperator
```
join(",", ["1", "2", "3", "4", "5"])
```