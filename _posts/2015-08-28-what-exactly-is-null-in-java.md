---
layout: post
title: Java中的 null 到底是什么？
tagline: "what exactly is null in java"
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

先看下面的声明语句

```java
String = null;
```

这个声明语句到底做了什么

回想一下，什么是变量？什么是值？一个常见的比喻就是：变量就像是一个盒子，你可以用盒子来存放东西，你可以用变量来存放值。当声明一个变量，我们需要设置它的类型。

Java中主要有两大类：基本数据类型和引用。声明为基本数据类型的变量存储值，声明为引用类型的变量存储引用。上面的表达式中，初始化语句声明一个变量“x”。“x”存储字符串引用。在这里是 null。

看下图可以更清楚的理解这个概念。

![pic1](/assets/images/what-is-null-150x150.png)


如果是 x = "abc"，会像下图所示：

![pic2](/assets/images/variable-reference-300x144.png)

###null在内存中是什么？

null在内存中是什么？或者说Java中的 null是什么？

首先，null不是一个有效的对象实例，所以并没有给它分配内存。它是一个表示对象的引用还没有指向一个对象的值。

[JVM规范](http://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.4)中：

>The Java Virtual Machine specification does not mandate a concrete value encoding null.
>Java虚拟机规范并没有强制要求一个具体的值编码为空
x 在内存里是什么？

现在我们知道了 null是什么。我们也知道变量是一个储存位置相关的符号名（标示符），它包含了值，x在内存中是什么？

从JVM 运行时数据区的图看来，因为每个方法在线程栈 (thread'stack)中都有私人栈帧 (private stack frame)，局部变量就位于这个栈帧中。
