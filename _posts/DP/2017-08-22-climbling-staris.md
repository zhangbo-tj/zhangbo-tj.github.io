---
layout: post
title: 爬楼梯
category: 动态规划
tags: Dynamic-Programming LeetCode
Keywords: Dynamic-Programming
description:
---
## 1. LeetCode 70. [Climbling Stairs](https://leetcode.com/problems/climbing-stairs/description/)
## 2. 描述：
假设你正在爬楼梯，楼梯共有n个台阶。 每次只能向上爬一个或两个台阶，那么总共有多少种不同的方式可以到达楼梯顶部。
## 3. 解题思路：
假设现在爬到了第i阶楼梯，那么此时有两种可能可以到达该台阶：
1. 在第i-1阶台阶处，爬了1个台阶
2. 在第i-2阶台阶处，爬了2个台阶
因此用 D[i]表示到达第i阶台阶的不同方式的话，可以很容易地得出： D[i] = D[i-1] + D[i-2];
## 4. 解决方案：
``` c++
int climbStairs(int n) {
    vector<int> nums(n+1,0);
    nums[0] = 1;
    nums[1] = 1;
    for(int i = 2; i <= n; i++){
        nums[i] = nums[i-1] + nums[i-2];
    }
    return nums[n];
}
```
