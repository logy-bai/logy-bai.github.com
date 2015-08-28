---
layout: post
title: Java中字符串的引用传递
tagline: "string is passed by reference in java"
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

这是Java中的一个经典问题，stackoverflow网站上有很多类似的问题和很多不正确/不完整的答案。如果你不去多想，这个问题很简单，但是如果你多思考一会，你会觉得这个问题很令人困惑。

###下面的一段代码既令人感兴趣又让人困惑。

```java
public static void main(String[] args) {
    String x = new String("ab");
    change(x);
    System.out.println(x);
}
  
public static void change(String x) {
    x = "cd";
}
``` 
输出：

	ab

在c++中，代码如下：

```c++
void change(string &x) {
    x = "cd";
}
  
int main(){
    string x = "ab";
    change(x);
    cout << x << endl;
}
```
输出为：

	cd

###常见的令人困惑的问题（高能预警！以下是错误解释！）


x储存一个指向内存堆中的字符串的引用，所以当x被当做参数传递给change()方法时，它依然指向这内存中的”ab”如下图

![pic1](/assets/images/string-pass-by-reference--650x247.jpeg)

因为Java是按值传递的，x的值是指向”ab”的引用，当change()方法被调用，它创建了新的对象”cd”，x现在就指向了字符串对象”cd”,如下图：

![pic2](/assets/images/string-pass-by-reference-2-650x247.jpeg)

这似乎是一个非常合理的解释。他们明确表示，Java是永远传递按值。但是，什么是错在这里？


###程序究竟是怎么运行的！！！


当字符串”ab”被创建，Java分配内存来储存字符串对象。然后这个对象分配给了变量x，其实变量x只是被分配了对象的引用，并非对象本身。引用就是对象被储存在内存中的地址。

 

变量x包含字符串对象的引用，x不是引用，它是一个变量，只不过储存了一个引用（内存地址）（被绕晕了没…）

Java只能按值传递，当x传递给了change()方法，一个x变量的副本被传递过去了，change()方法创建另一个对象”cd”，它有着和前者不同的引用，是这个变量x改变了把原本的引用改成了指向对象”cd”的引用，并不是改变了引用本身。（我快翻译哭了，原博主解释能力捉急%>_<%）

下图能解释到底发生了什么。

![pic3](/assets/images/string-pass-by-reference-3-650x244.jpeg)


###错误解释


这个问题跟前面讲到的字符串对象的不变性没有任何关系。就算是使用StringBuilder来创建，运行结果还是一样的。问题的关键在于储存引用的变量，而不是引用本身！！！


###解决问题的方法


首先，对象必须是可改变的，如StringBuilding。第二，要确保没有新对象被创建并分配给了参数变量，因为Java是按值传递的。

```java
public static void main(String[] args) {
    StringBuilder x = new StringBuilder("ab");
    change(x);
    System.out.println(x);
}
  
public static void change(StringBuilder x) {
    x.delete(0, 2).append("cd");
}
``` 

