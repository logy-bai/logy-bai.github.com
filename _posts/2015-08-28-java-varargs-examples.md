---
layout: post
title: Java可变参数
tagline: "java varargs examples"
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

可变参数（可变参数）是在Java 1.5中引入的功能。它允许一个方法使用任意数量的值作为参数。

```java
public static void main(String[] args) {
    print("a");
    print("a", "b");
    print("a", "b", "c");
}
public static void print(String ... s){
    for(String a: s)
        System.out.println(a);
}
```

###可变参数是怎样工作的？

当使用可变参数功能时，实际上会先创建一个数组，大小为调用位置传过来参数的数量。然后把参数的值放进数组，最后把数组当做参数传进方法（method）中。

###什么时候使用可变参数？

作为其定义，当方法需要处理任意数量的对象时，可变参数是非常有用的。Java SDK中的 String.format(String format, Object... args) 就是一个很好的例子。字符串可以格式化任何数目的参数，使用了可变参数。

```java
String.format("An integer: %d", i);
String.format("An integer: %d and a string: %s", i, s);
```

