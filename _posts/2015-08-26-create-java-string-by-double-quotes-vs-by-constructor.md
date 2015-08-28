---
layout: post
tagline: create java string by double quotes vs by constructor
title: "创建字符串，用双引号“ ” 还是 用构造器（Constructor）"
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

在Java中，创建字符串有两种方式：

```java
String x = "abc";
String y = new String("abc");
```
  
那么问题来了，创建字符串到底哪家强？

###双引号 vs 构造器


这个问题可以有下面两个简单的例子回答

```java
String a = "abcd";
String b = "abcd";
System.out.println(a == b);  // True
System.out.println(a.equals(b)); // True
````
  
a == b 返回true是因为 a 和 b 引用了方法区类同一个字符串。内存引用是相同的，当同样的字符串多次被创建，只有一个字符串的值被储存，这就是所谓的“字符串驻留”，Java中所有编译期间的常量字符串将会自动驻留。

```java
String c = new String("abcd");
String d = new String("abcd");
System.out.println(c == d);  // False
System.out.println(c.equals(d)); // True
```
  
c == d 返回false因为 c 和 d 引用了内存堆中的不同对象，不同的对象有不同的内存引用。

下面的图可以说明上面的原因：

![pic1](/assets/images/constructor-vs-double-quotes-Java-String-New-Page-650x324.png) 

###运行期（Run-Time）字符串驻留


字符串驻留也会发生在运行期，即使两个字符串都是使用构造器创建：

```java
String c = new String("abcd").intern();
String d = new String("abcd").intern();
System.out.println(c == d);  // Now true
System.out.println(c.equals(d)); // True
````

###什么时候该使用哪一种呢？


因为字符串“abcd”已经是字符串类型了，使用构造器会创建一个额外不必要的对象，因此，你应该使用双引号来创建字符串。

如果你真的需要在内存堆中创建一个新的对象，就应该使用构造器。