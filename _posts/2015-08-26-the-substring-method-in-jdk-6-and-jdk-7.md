---
layout: post
title: "JDK6和JDK7中substring() 方法的区别"
tagline: "The substring Method in JDK 6 and JDK 7"
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

JDK6和JDK7中的 substring(int beginIndex，int endIndex)方法是不同的，了解这些能帮助我们更好的使用它们，简单起见，下文中用 substring()代替 substrings(int beginIndex,int endIndex)

###substring()方法是做什么用的？

其实substring(int beginIndex,int endIndex)方法就是返回一个从beginIndex开始到endIndex-1结束的字符串.

```java
String x = "abcdef";
x = x.substring(1,3);
System.out.println(x);
```

输出：

	bc

###当substring()被调用时，究竟发生了什么！！！

你可能认为（但事实并非如此！），因为字符串对象的不变性（详见文章6、字符串的不变性），当x被赋值为x.substring(1,3)时，x储存的引用将会指向一个新的字符串对象（string对象），如下图所示：

![pic1](/assets/images/string-immutability1-650x303.jpeg)

然而，这个图并没能准确的表示出内存上（堆，the heap）上发生了什么，当substring()方法被调用时，到底发生了什么呢，在JDK6和JDK7上又有什么不同呢？


###JDK6中的substring()方法

字符串是通过字符数组实现的，在JDK6中，字符串类（the string class）包含3个字段（即数据成员）：char value[] , int offset , int count . 它们分别用来储存真实的字符数组（char value[]）、第一个字符所在原字符数组的偏移量（int offset）、字符串中字符的数量（int count）。

![pic2](/assets/images/string-substring-jdk6-650x389.jpeg)

为了解释这个问题的关键点，下面是字符串的简化代码：

```java
//JDK 6
 
String(int offset, int count, char value[]) {
  this.value = value;
  this.offset = offset;
  this.count = count;
}
  
public String substring(int beginIndex, int endIndex) {
  //check boundary
    return  new String(offset + beginIndex, endIndex - beginIndex, value);
}
```

###JDK6中的substring()方法会导致一个问题！

如果你有一个特别长的字符串，但你每次只需要很短的一部分，所以你使用substring()方法，这就会导致一个性能问题，因为你只需要很小的一部分，substring()方法得到的字符串对象中的char value[]字段却保留了父数组的全部字符数组（也就是导致了空间浪费）。所以对于JDK6，解决方法如下：

```java
x = x.substring(a, b) + ""
```
###JDK7中的substring()方法

这个问题在JDK7中得到了改善，JDK7中的substring()方法会再内存堆里创建一个新的字符数组。

![pic3](/assets/images/string-substring-jdk71-650x389.jpeg)

```java
//JDK 7
public String(char value[], int offset, int count) {
    //check boundary
    this.value = Arrays.copyOfRange(value, offset, offset + count);
}
  
public String substring(int beginIndex, int endIndex) {
    //check boundary
    int subLen = endIndex - beginIndex;
    return new String(value, beginIndex, subLen);
}
```
