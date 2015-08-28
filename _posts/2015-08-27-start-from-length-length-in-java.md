---
layout: post
title: Java中的 length 和 length()
tagline: "start from length length in java"
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

首先，你能快速回答下面的问题吗？！

>Without code autocompletion of any IDE, how to get the length of an array? And how to get the length of a String?

>在没有代码自动补全的IDE工具的情况下，怎么得到数组的长度？又怎么得到字符串的长度？

我向不同级别的开发者问过这个问题：包括入门的和中级的开发者。他们都不能准确且自信的回答这个问题。IDE工具提供了方便的代码自动补全功能，这也导致了我们停留在表面上的理解。在这篇文章里，我将解释关于Java数组的一些关键概念。

答案：

```java
int[] arr = new int[3];
System.out.println(arr.length);//length for array 
String str = "abc";
System.out.println(str.length());//length() for string
```

问题是，为什么数组有 length 字段，而字符串却没有呢？或者为什么字符串有 length() 方法，而数组却没有呢？

###问题 1 为什么数组有 length 属性？

首先，数组是一个保存固定个数的单一类型值的容器对象，数组一旦被创建后，它的长度就不会在改变。数组的长度可以作为最终实例的长度。因此，length 可以看做是一个数组的定义属性。

有两种方法创建数组：
1、数组创建表达式
2、数组初始化

当数组被创建，大小就被指定了。

上面的例子使用的就是数组表达式。它指定元素的类型，维数，和至少一个维度的长度（length）。

下面的数组的声明是合法的，因为它指定了一个维度的长度。

```java
int[][] arr = new int[3][];
```

数组初始化创建数组并给予所有的初始值。这是由逗号分隔的表达式，用花括号{}括起来。

```java
int[] arr = {1,2,3};
```

###问题 2 为什么不定义一个像字符串的数组类

因为数组是一个对象，下面的代码是合法的。

```java
Object obj = new int[10];
```

数组包含了所有继承自Object类（除克隆）的成员。为什么没有定义一个数组的类呢？我们无法找到Array.java文件。粗略的解释是，他们被隐藏了。你可以想想这个问题 - 如果有数组的类，会是什么模样？它仍然需要一个数组来保存数组数据。所以不定义这样一个类。

我们可以用下面的代码得到数组的类：

```java
int[] arr = new int[3];
System.out.println(arr.getClass());
```

	Output:class [I

"class [I"代表int型数组再运行时的类型签名。

###问题 3 为什么 String 有 length() 方法？

字符串的后备的数据结构是字符数组，没有必要为每一个字符串定义一个 length 属性。不像C语言，在Java中字符数组不是字符串。

参考：

[1] [Arrays](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)

[2] [JLS Array](http://docs.oracle.com/javase/specs/jls/se7/html/jls-10.html#jls-10.7)
