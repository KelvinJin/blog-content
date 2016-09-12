---
title: 'C# 基础'
permalink: c-ji-chu
id: 22
updated: '2015-03-12 18:46:55'
tags:
---

终于开始学习C#，感觉有点晚啊。

- C#也有类似Java的虚拟机叫做CLR（公共语言运行时 Common Language Runtime）。而这种虚拟机能够识别的语言被称为IL（中间语言 Intermediate Lauguage）
- （一个我之前不知道的常识）C#在进行类型转换的时候分两种情况，如果是精度增加的转换，比如int到long或者float到double，系统会自动完成。而如果是精度减少的转换，必须使用强制转换（在前面加类型）。其实这么做是为了保证不会出现如果转换前的数大于了转换后类型的最大值，而造成结果非数字但是程序员又没有意识到的情况。（这样多好，不像Swift一样，到处都要转化。。。。）
- C#里面的String是引用类型（这点和Swift不一样）。但是呢，当你用 "=="来比较两个String的时候，又实际上是在比较两个String的内容，这点呢，又和C#一般的引用类型不一样，这是因为String重载了"=="运算符。（其实这点又和Swift不一样，Swift里面"=="是会比较内容的，比较是否是同一块内存要用"==="）
- C#的构造函数也需要把所有的成员变量都初始化。。。（这点听上去似乎比Swift还严格）
- Struct作为函数的变量可以保证多线程的并行不冲突（这个倒是需要权衡，如果老是进行Copy的话，性能也会受到很大影响）
- 驻留池（Intern Pool）是一个用来存放代码里面所有文本常量的地方。而任何动态创建的都不会放在里面（拐了弯的都不会）。
- 大量String的加减操作时，由于会产生很多Box，Concat，复制等操作，会大大影响性能，这时候可以考虑用StringBuilder预先分配空间。
- C#里面也有和Swift类似的Optional类型，也是直接加?，比如Int?。转换也可以使用as关键字（话说这是Swift抄袭C#的吧。。。）。可空类型是值类型。"??"也是有的。
- C#中的泛型也是可以深入研究的。正确的应用泛型可以提高程序的性能。

- 对象与集合初始化器。。。这个有点意思。
- C#里面函数参数也可以又可变型，类似Swift里面的inout关键字。out关键字表示只出不进，有点像iOS里面我们用NSError的时候不需要初始化，直接给地址过去，如果有错误了，函数会负责初始化并赋值。而如果要像Swift里面那样同时传值进去就要用到ref。（Swift又抄袭了一个。。。）
- C#里面也有命名参数（这个也许是ObjctiveC先有的？），因此可以实现可选参数。（类似Swift）
- C#里面提供partial 关键字来将同一个类的定义放到同一个项目的不同文件中。可惜Swift暂时还不支持。。。不过也不好说这个是好是坏，感觉会增加项目结构的复杂性，除非编辑器支持好。
- Partial关键字还支持分部方法，也就是说可以在一个文件中声明，另一个文件中定义。最重要的是，如果只提供了声明而没有定义，程序编译时会将该方法的使用全部删除，不会影响程序运行。这给动态植入方法提供了可能，特别是在测试一些可能的实现而又不想破坏现有的函数结构的时候。
- C#支持类扩展（类似ObjC的Category或者Swift的Extension）。但是在写法上，C#的扩展是以静态类包装静态方法的方式，每个方法的第一个参数用来指定基础类。
```
class MyClass 
{
	public int value = 100;
}

// 扩展
static class MyClassExtension 
{
	public static int ExtensionMethod1(this MyClass nouse, int a, int b)
    {
    	return a + b
    }
}
```
C#规定如果在扩展类中有和基础类相同名称和签名的方法，该方法会始终被原始方法替代，但是如果用命名空间的方法来限制？

- 有趣的是，Objective C的Category实际上是在runtime将方法动态的添加到类当中去，而不仅仅是[Syntax sugar for static method](http://programmers.stackexchange.com/questions/218633/are-extension-methods-c-and-categories-objective-c-the-same-as-traits).

- 从扩展方法的定义可以看出，如果扩展方法本身是泛型的，那么我们就将此方法应用在不同类上。相当于一次给很多类同时写了扩展。
- C#的JIT编译器会进行及时编译，第一次调用方法的时候会生成对应当前CPU的机器码然后缓存起来，而第二次调用的时候就会直接使用缓存方法。所以第一次以后的调用速度都会加快。（这么说是不是程序初始阶段应该有空就调用一下各种方法来预编译啊。。。）

- C# 里面有个东西叫做依赖属性(dependency property)。这个东西有点像Swift 里面的associated object，也是可以set, get。依赖属性有个很重要的特性是自动通知，因此可以作为data binding的一个元素。包含依赖属性的类需要继承自DependencyObject才能拥有GetValue和SetValue两个方法来访问属性的值。

- .Net 有个组成叫做WPF，用来制作各种Windows窗体应用（代码有点像XML），而这其中有些属性是可以继承的（这里的“属性“和HTML里面的属性是相同意思），其中DataContext就是可以集成的，而DataContext又可以指定为某个对象的实例从而可以使用这个对象实例的各种属性（包括Dependency property）来实现Data binding。

- Metadata是依赖属性的一个重要特性，看看下面的定义就知道这玩意有什么用处了。两个回调函数用来做validation。其中DependencyObject 还有一个ClearValue用来把Dependency Property恢复默认值。
![依赖属性的Metadata](/content/images/2015/03/Screen-Shot-2015-03-12-at-2-56-05-pm.png)
