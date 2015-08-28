---
layout: post
title: "图解Java 字符串的不变性"
tagline: diagram-to-show-java-strings-immutability
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

本篇文章用一组图表来说明Java的String不变性。

###声明一个字符串

```java
String s = "abcd";
```

变量s储存字符串对象的引用，下面的箭头应该解释为‘字符串变量s储存字符串'abcd'的引用’。

![pic1](/assets/images/String-Immutability-1.jpeg)

###把一个字符变量复制给另一个字符变量

```java
String s2 = s;
```
S2也储存同样的引用，因为引用了同样的字符串对象。

![pic2](/assets/images/String-Immutability-2.jpeg)

###拼接字符串

```java
s = s.concat("ef");
```

s现在储存新建字符串对象的引用。

![pic3](/assets/images/String-Immutability-650x279.jpeg)

##总结

 一旦字符串对象在内存（堆）中被创建，就不能再被改变。所有字符串方法都不能改变字符串对象，但是可以返回一个新建的字符串对象。

如果你需要一个可以被修改的字符串，则需要使用StringBuffer或者StringBuilder。否则，将会大量的时间浪费在垃圾回收上（Garbage Collection），因为每次都会创建一个新的String对象在内存上。

