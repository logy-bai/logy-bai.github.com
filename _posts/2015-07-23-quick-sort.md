---
layout: post
title: "快速排序思想及Java实现"
description: ""
category: Algorithm
tags: [Algorithm,quick sort,sort]
---
{% include JB/setup %}

## 快速排序
快速排序是最常用的排序算法之一，相比于时间复杂度为O(N<sup>2</sup>)的冒泡排序算法，快速排序的时间复杂度只是```O(NlogN)```。

## 算法思想
快速排序的算法思想其实比较简单，先找出一个数作为基准数（这里取数组最中间的一位）。定义两个变量作为“哨兵”，然后分别从后向前，从前向后两个方向去“探测”：

1. 从后向前：寻找比基准数小的数据，如果找到，停下来
2. 从前向后：寻找比基准数大的数据，如果找到，停下来
3. 如果两个方向的“探测”都找到了符合要求的数据，则交换数据，继续顺着方向寻找
4. 直到两个哨兵碰到一起，此时把相遇位置上的数据和基准数（即数组的中间位）交换数据
5. 此时，基准数左侧的数都小于等于基准数，右侧的数都大于等于基准数
6. 同样的方法去“探测”基准数左侧和右侧的数据（使用递归）


比如有下面一个数组:

	6，1，2，7，9，3，4，5，10，8	

我们需要自己定义几个变量：
* low : 数组的最低位
* high : 数组的最高位
* pivot : 数组的基准位

quickSort(low=0,high=9)
	6, 1, 2, 7, 9, 3, 4, 5, 10, 8
	↑           ↑               ↑
	i         pivot             j

	numbers[i] < numbers[pivot]
		i++

        6, 1, 2, 7, 9, 3, 4, 5, 10, 8
                    ↑               ↑
                  pivot             j
		    ↑
		    i
	numbers[i] < numbers[pivot]
            exchange(i,j)

        6, 1, 2, 7, 8, 3, 4, 5, 10, 9
                    ↑               ↑
                    i               j

        6, 1, 2, 7, 8, 3, 4, 5, 10, 9
                                 ↑  ↑
                                 i  j

        6, 1, 2, 7, 8, 3, 4, 5, 10, 9
                             ↑   ↑
                             j   i

		   i>j
		不交换数据
	       此时i=8,j=7

使用递归分别执行
quicksort(low,high=i)
quicksort(low=j,high)

##Java实现
```java
public class Quicksort  {
  private int[] numbers;
  private int number;

  public void sort(int[] values) {
    // 检查数组是否为空
    if (values ==null || values.length==0){
      return;
    }
    this.numbers = values;
    number = values.length;
    quicksort(0, number - 1);
  }

  private void quicksort(int low, int high) {
    int i = low, j = high;
    // 把数组中间的元素设置为基准数
    int pivot = numbers[low + (high-low)/2];

    // 分开成两个数组
    while (i <= j) {
    //从左向右“探测”，如果左边的元素小于基准数，则去“探测”下一个元素
      while (numbers[i] < pivot) {
        i++;
      }
      //从右向左“探测”，如果左边的元素大于基准数，则去“探测”下一个元素
      while (numbers[j] > pivot) {
        j--;
      }

      // 如果左边探测结果大于基准数，右边探测结果小于基准数，那么交换这两个元素
      // 然后继续探测
      if (i <= j) {
        exchange(i, j);
        i++;
        j--;
      }
    }
    // 递归
    if (low < j)
      quicksort(low, j);
    if (i < high)
      quicksort(i, high);
  }

  private void exchange(int i, int j) {
    int temp = numbers[i];
    numbers[i] = numbers[j];
    numbers[j] = temp;
  }
} 

```


******
