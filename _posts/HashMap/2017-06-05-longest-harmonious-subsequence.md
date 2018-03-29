---
layout: post
title: 最长和谐子序列
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 594: [Longest Harmonious Subsequence](https://leetcode.com/problems/minimum-index-sum-of-two-lists/description/)
## 2. 描述：
和谐数组是指一个数组中最大值和最小值的差为1。

给定一个整数数组，需要找出其子数组中最长的"和谐序列"的长度
## 3. 示例：
```
Input: [1,3,2,2,5,2,3,7]
Output: 5
Explanation: The longest harmonious subsequence is [3,2,2,2,3].
```
## 4. Solution 1:
``` c++
int findLHS(vector<int>& nums) {
    unordered_map<int, int> num_map;
    for(auto num: nums){
        num_map[num]++;
    }
    int result = 0;
    for(pair<int, int> key_value: num_map){
        if(num_map.find(key_value.first + 1) != num_map.end()){
            result = std::max(result, key_value.second + num_map[key_value.first + 1]);
        }
    }
    return result;
}
```