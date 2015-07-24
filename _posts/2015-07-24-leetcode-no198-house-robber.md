---
layout: post
title: "leetcode No.198 House Robber"
description: ""
category: Algorithm
tags: [Dynamic Programming, Algorithm]
---
{% include JB/setup %}

##题目内容

>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

>Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

****
##解题思路
本题采用动态规划思想：```抢劫n间房子的金钱总和＝MAX( 抢劫n-2间房子的金钱总和＋第n间房子的金钱 , 抢劫n-1间房子的金钱总和 )```

****
##Java代码

```java

public class Solution {
    public int rob(int[] num) {
	//如果总共有0个房子，则返回0
    	if(num.length==0)
    		return 0;
	//如果总共有1个房子，那么返回这个房子里的金钱
    	if(num.length==1)
    		return num[0];
	//如果总共有两个房子，那么抢劫钱多的那个房子
    	if(num.length==2)
    		return Math.max(num[0], num[1]);
	//如果房屋数大于2
	//先分别算出只有一间房子时抢劫金钱数prior
	//和只有两间房子时的抢劫金钱数next
	//prior可能等于next```想想为什么```
        int i=0;
    	int prior=num[0];
    	int temp=0;
    	int next=Math.max(num[0],num[1]);
    	//当有三间房子的时候
	//抢劫金钱最大数要么等于只有两间房子时的最大数
	//要么等于只有一间房子的金钱数＋第三间房子的金钱数
	//以此类推4间房子、5间房子、等等
        for(i=3;i<=num.length;i++){
        	temp=prior;
        	prior=next;
        	next=Math.max(temp+num[i-1], prior);
        }
        return Math.max(prior, next);
    }
    
}

```

*********
