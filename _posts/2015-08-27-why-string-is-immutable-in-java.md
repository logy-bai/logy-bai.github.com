---
layout: post
tagline: why string is immutable in java
title: "为什么Java中的字符串具有不可变性?"
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

java中的字符串（string）是一个不可变的类，不可变的类就是这个类的实例不能被修改，当实例被创建后，这个实例的所有信息都被初始化，这些信息将不能再被修改。不可变的类有很多优点。本文总结为什么字符串类被设计为不可变的类。要回答这个问题需要深入理解内存，同步，数据结构等知识。

###字符串池（string pool）

字符串池是方法区（Method Area，这又是什么鬼？）中的一个特殊的储存区域。当创建一个字符串时，如果这个字符串之前就已经存在于字符串池中，那么就会返回这个之前就已经存在字符串的引用，而不是去创建一个新的实例对象并返回引用。

下面的代码只会创建一个字符串实例对象在内存堆中

```java
String string1 = "abcd";
String string2 = "abcd";
```

![pic1](/assets/images/java-string-pool.jpeg)


###缓存哈希码（caching hashcode）

字符串的hashcode在Java中很用，如HashMap。具有了不可变性就保证了hashcode不会变化，所以就可以缓存hashcode而不必担心它会发生。这就意味着，当不可变类的实例对象每次被使用就不需要再去计算它的hashcode。这样更有效率。

在字符串类里，有下面的代码：

```java
private int hash;//this is used to cache hash code.
```

###方便其他类的使用

为了更具体的理解，请看下面的程序：

```java
HashSet<String> set = new HashSet<String>();
set.add(new String("a"));
set.add(new String("b"));
set.add(new String("c"));
 
for(String a: set)
    a.value = "a";
```

在这个例子中，如果字符串类的实例是可变的，那么这些值就会被改变进而违反了hashset的设计原则（hashset只包含不重复的元素）。这个例子只是为了简单理解，实际上真正的字符串类没有value字段。（就是说a.value是语法错误的，作者为了让大家理解自己造的…实际上上面的代码执行后并没有改变set里的值，输出的仍然是abc。不知道这样说大家理解了没。）

###安全性

在很多java类中，字符串被广泛的当做参数，比如网络连接，打开文件等等类，如果字符串不具有不变性，上述的网络连接，文件将会改变，导致一系列的安全问题。该方法认为它连接到一台电脑，但其实并不是这样的。可变的字符串同样会导致安全问题映射，因为这些参数都是字符串。

  下面是例子：

```java
boolean connect(string s){
    if (!isSecure(s)) { 
        throw new SecurityException(); 
    }
//here will cause problem, if s is changed before this by using other references. 
//如果在这之前通过使用其他的引用改变了s的值，那这里就会出现问题   
    causeProblem(s);
}
```

###不可变的对象是线程安全的

因为不可变的对象是不能被修改的，所以他们就能在多个线程之间自由的共享。这就从根本上消除了在不同线程之间做同步的需求。


总之，字符串为了效率和安全的原因被设计成不可变的，这也是为什么一般情况下不可变类更受青睐。
  