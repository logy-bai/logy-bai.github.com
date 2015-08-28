---
layout: post
title: 怎样更有效率的检查一个数组是否包含某个元素
tagline: "check if array contains a value java"
description: ""
category: Java
tags: [Java,翻译]
---
{% include JB/setup %}

怎样去检验一个数组（未排序）是否包含某个元素？这是Java中的一个非常有用和非常常用的一个操作。这也是stackoverflow网站上的一个投票很高的问题。在投票很高的几个回答中，有很多不同的方法，但是时间复杂度也是不同的，下面我将介绍每种方法的时间成本。

###检查数组是否包含某个元素的四种不同的方法
  
  1）使用 List:
  
```java
public static boolean useList(String[] arr, String targetValue) {
    return Arrays.asList(arr).contains(targetValue);
}
```

  2）使用 Set:


```java
public static boolean useSet(String[] arr, String targetValue) {
    Set<String> set = new HashSet<String>(Arrays.asList(arr));
    return set.contains(targetValue);
}
```

  3）使用简单的循环：


```java
public static boolean useLoop(String[] arr, String targetValue) {
    for(String s: arr){
        if(s.equals(targetValue))
            return true;
    }
    return false;
}
```

4）使用 Arrays.binarySearch():

下面的代码是错误的，binarySearch()只能适用于已经排过序的数组。运行下面的代码结果会很奇怪。

```java
public static boolean useArraysBinarySearch(String[] arr, String targetValue) { 
    int a =  Arrays.binarySearch(arr, targetValue);
    if(a > 0)
        return true;
    else
        return false;
}
```

###时间复杂度

需要的时间可以通过使用以下代码来测量。其基本思路是，以搜寻大小分别为5、1000、10000的数组。该方法可能不准确，但这个想法是简单明了的。

使用大小为5的数组：

```java
public static void main(String[] args) {
    //use list
    String[] arr = new String[] {"CD","BC","EF","DE","AB"};
        long startTime = System.nanoTime();
    for (int i = 0; i < 100000; i++) {
        useList(arr, "A");
    }
    long endTime = System.nanoTime();
    long duration = endTime - startTime;
     //use set
    System.out.println("useList:  " + duration / 1000000);
    startTime = System.nanoTime();
    for (int i = 0; i < 100000; i++) {
        useSet(arr, "A");
    }
    endTime = System.nanoTime();
    duration = endTime - startTime;
    //use loop
    System.out.println("useSet:  " + duration / 1000000);   
    startTime = System.nanoTime();
    for (int i = 0; i < 100000; i++) {
        useLoop(arr, "A");
    }
    endTime = System.nanoTime();
    duration = endTime - startTime;
    //use Arrays.binarySearch()
    System.out.println("useLoop:  " + duration / 1000000);  
    startTime = System.nanoTime();
    for (int i = 0; i < 100000; i++) {
        useArraysBinarySearch(arr, "A");
    }
    endTime = System.nanoTime();
    duration = endTime - startTime;
    System.out.println("useArrayBinary:  " + duration / 1000000);
}
```

运行结果：


	useList:  13
	useSet:  72
	useLoop:  5
	useArraysBinarySearch:  9

使用大小为1000的数组：

```java
String[] arr = new String[1000];
Random s = new Random();
for(int i=0; i< 1000; i++){
    arr[i] = String.valueOf(s.nextInt());
}
```

运行结果：

	useList:  112
	useSet:  2055
	useLoop:  99
	useArrayBinary:  12

使用大小为10000的数组：

```java
String[] arr = new String[10000];
Random s = new Random();
for(int i=0; i< 10000; i++){
    arr[i] = String.valueOf(s.nextInt());
}
```

运行结果：

	useList:  1590
	useSet:  23819
	useLoop:  1526
	useArrayBinary:  12

很显然，使用循环的方法任何Collection的效率高，很多程序员使用第一种（使用 List）,其实效率并不好。把数组放入另一个Collection，在使用Collection处理数据前，需要先把数组中的每一个元素读进Collection中。

在使用 Arrays.binarySearch()方法之前，数组必须是已经排过序的。本文中的数组没有排序，所以不应该使用它。

其实，如果你真的需要高效率的检查 数组/Collection 中是否包含某个值，已排序的list或树可以做到在时间复杂度为O(log(N))，HashSet的可以在时间复杂度为O(1)做到这一点。

