---
title: iOS Swift Basic Data type and KVO notes
permalink: notes-about-ios
id: 29
updated: '2014-12-09 16:10:26'
date: 2014-11-20 14:04:44
tags:
---

#####In Swift

- `NSInteger` is actually `Int`
- `CGFloat` is `Float` in 32-bit architecture and `Double` in 64-bit architecture
- `Int` is `Int32` in 32-bit architecture and `Int64` in 64-bit architecture. This is quite different in other language where Int is always Int32 (such as C#)
- `Float` is `Float32` on both 32-bit and 64-bit device, `Double` is `Float64`.
- `INT_MAX` in Swift is actually `Int32.max` on both 32-bit and 64-bit device while `Int.max` is `Int64.max` on 64-bit device and `Int32.max` on 32-bit device. This is different from Objc.

The performances of the following codes are so different!
```
let start = CFAbsoluteTimeGetCurrent()
for _ in 0...1000000 {
}
let timeTaken = CFAbsoluteTimeGetCurrent() - start
println(timeTaken) // 0.17

let start = CFAbsoluteTimeGetCurrent()
for var i = 0; i < 1000000; i++ {
}
let timeTaken = CFAbsoluteTimeGetCurrent() - start
println(timeTaken) // 0.0032
```

Nearly 100 times faster.

Two `UInt` numbers cannot substract directly:

```Swift
let a: UInt = 0
let b: UInt = 1
let c = a - b // WRONG!!!
```

#####KVO

Key Value Observing

- `Key` is the name of the property whereas `Key Path` is concatenation of `Keys`. Say `address` is a `Key` for a `Value` object while `address.street` is a `Key Path`.
- `valueForKey`, `valueForKeyPath` and `dictionaryWithValuesForKeys` are used to retrieve specific value(s) for key(s). Similar to getters
- `NSNull()` can be used as an instance to add to `NSArray`, `NSDictionary` where you cannot directly use `nil`
- `accounts.transactions.payee` will return all the `payee` in all the `transations` in all `accounts`. **This is perfect if you have multiple one-to-many relations** (Think about it in web development, similar to `accounts.map { |x| x.transations }.flatten.map { |x| x.payee }.flatten ` but much simplier)
- `setValueForKey`, `setValueForKeyPath` and `setValuesForKeysWithDictionary` are used to set value(s) for key(s). Similar to setters.

In Swift:

```
class MyStruct: NSObject {
    var changableObject = NSFileManager.defaultManager()
    var changableAndNilableObject: NSFileManager? = NSFileManager.defaultManager()
    let unchangableObject = NSFileManager.defaultManager()
}
let myStruct = MyStruct()
```
If we do
```
// Throw error since it's constant.
myStruct.setValue(NSNull(), forKey: "unchangableObject")
// Throw error since it's not optional variable
myStruct.setValue(nil, forKey: "changableObject")
```

But if we do
```
// This works...Since it's NSNull is a singleton class which represents nil object in Collection objects.
myStruct.setValue(NSNull(), forKey: "changableObject")

// This works. It's nilable.
myStruct.setValue(nil, forKey: "changableAndNilableObject")
```
If the property doesn't have a setter, the above setValue will fail as well.

- Implement `setNilValueForKey` to set the default value for the key.
- Use `mutableArrayValueForKey` if you want to change the collection type property of the receiver:
```
class MyStruct: NSObject {
    var arrays = [1, 2, 3, 4, 5]
}
let myStruct = MyStruct()
var mutable = myStruct.mutableArrayValueForKey("arrays")
var immutable = myStruct.valueForKey("arrays")

immutable[2] = 4 // myStruct.arrays is still [1, 2, 3, 4, 5]
mutable[2] = 4 // myStruct.arrays now is [1, 2, 4, 4, 5]
```