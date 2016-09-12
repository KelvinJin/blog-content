---
title: Java Class Notes
permalink: untitled-10
id: 18
updated: '2015-01-27 22:09:17'
date: 2015-01-27 22:08:32
tags:
---

最近突然要写Java，有好多基础都忘了，在这里记录一下：

#### 类的构造函数问题

+ 一个类如果没有人为定义任何构造函数，则在创建对象时 _（new Class()）_ 会自动调用 **默认构造函数** 。 
  其中，int 类型会自动赋值 0；
 	char 类型会自动赋值 '\0';
  object 会自动赋值 null;

+ 一个类如果定义了 **一个或多个有参或无参构造函数** ，则无法生成 **默认构造函数** 。因此，如果一个类 _仅仅_ 定义了 **有参构造函数**， 而在创建对象时并没有传递任何参数，则程序会出错。

+ 创建子类对象时，如果所调用的 **构造函数** （无论是默认的无参还是人为定义的无参或有参）没有在首行指定调用哪一个父类的构造函数，则系统会自动首先调用父类的 **无参构造函数** （无论是默认的还是人为的）。如果有指定调用某一个父类的构造函数则首先调用指定的。

+ 上面两点导致的后果就是：如果父类 _仅仅_ 定义了 **有参构造函数**， 而在创建子类对象时，所调用的 **构造函数** 没有在首行指定合格的父类构造函数，那么系统会调用父类的 **无参构造函数** （没有），找不到，会报错。

+ 注意，由于给父类函数设置 private 关键字的效果等同于对子类不可见，所以也会有相同的问题发生。

上面几点给我们的启示是，最好人为定义父类的无参构造函数，这样除非调用错误，否则一般可以避免错误的发生。

#### 继承问题（多态性）


```Java Father.java
package test;

public class Father {
    public int age;
    public char name;
    public Child child;
    public void getAge() {
        System.out.println("Father : getAge()");
    }

    private void getName() {
        System.out.println("Father : getName()");
    }

    public void getChild() {
        System.out.println("Father : getChild()");
    }
}

```

```Java Child.java
package test;

public class Child
    extends Father {

    public void getAge() {
        System.out.println("Child : getAge()");
    }

    public void getName() {
        System.out.println("Child : getName()");
    }

    public void getChild(int a) {
        System.out.println("Child : getChild(int a)");
    }
}
```

```Java ConstructorAna.java
package test;

public class ConstructorAna {
    public static void main(String args[]) {
        Father father = new Father();
        Father fatherChild = new Child();
        Child  child = new Child();
        father.getAge();
        father.getChild();
        
        fatherChild.getAge();
        fatherChild.getChild();
        // fatherChild.getName(); // Error
        // fatherChild.getChild(2); // Error
        
        child.getAge();
        child.getChild();
        child.getName();
        child.getChild(2);
    }
}
```

```Shell 输出结果
Father : getAge()
Father : getChild()
Child : getAge()
Father : getChild()
Child : getAge()
Father : getChild()
Child : getName()
Child : getChild(int a)
```

结论：

- 对象能够访问的 **函数名** （不是函数）完全由声明对象时前面的修饰符决定，如果是父类的实例就只能调用父类的函数（名）。之所以强调是 **函数名** 是因为如果子类进行了重写 @override，那么调用时调用的原函数还是重写函数不再取决于对象声明前的修饰符，而取决于后面的 new 什么。

- 父类中private的函数不能被重写（因为根本不可见），只能在子类中重新写，当然也就不可能直接通过实例调用无论是父类实例还是子类实例。

- 用子类修饰符定义的对象可以调用子类中所有函数，包括继承自父类的以及重载的（比如上面的getChild() 和 getChild(int a)）。


### 应用实例

上面的一些结论在实际中会导致很多问题。比如
父类A 		- 子类a
函数AA() 	- 函数AA() override
函数AB()	- 函数aB()

类B 中有一行{ A insA = new a(); }合法。
则：
- insA.AA()会调用 子类a 的 函数AA()。
- insA.AB()正常调用 父类A 的函数AB()。
- insA.aB()报错，父类中没有该函数。

其实我是希望能调用a中的新函数的。。。但是这个不是Java的问题，是我设计的问题。解决方法还在思考中。。。