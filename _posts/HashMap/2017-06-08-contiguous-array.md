---
layout: post
title: URL加密和解析
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 525: [Contiguous Array](https://leetcode.com/problems/contiguous-array/description/)
## 2. 描述：
给定一个只包含0和1的数组，在其所有连续的子数组中，找出0和1个数相等的最长子数组。
## 3. 示例 1：
```
Input: [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.
```
示例 2：
```
Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```
## 4. Solution：
``` c++
int findMaxLength(vector<int>& nums) {
    unordered_map<int,int> count_map;
    int count = 0;
    count_map[0] = -1;
    int max_len = 0;
    for(int i = 0; i < nums.size(); i++){
        count += (nums[i] == 0 ? -1 : 1);
        if(count_map.count(count)){
            max_len = max(max_len, i - count_map[count]);
        }else{
            count_map[count] = i;
        }
    }
    return max_len;
}
```
## 5. 解题思路：
count_map中保存的是<num, count>，即当前的和与index的对应关系，如果当前count == 2，而且之前已经保存过2了，那么在这二者之间的部分就是0和1的数量相等，因为如果为0就减一为1就加1。
