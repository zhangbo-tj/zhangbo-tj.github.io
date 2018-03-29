---
layout: post
title: 和为K的最长子数组
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 325:  Maximum size subarray sum equals k
https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/description/
## 2. 描述：
给定一个整数数组和一个目标值k,找出和为k的最长子数组的长度，如果不存在的话就返回0。
## 3. 示例 1：
给定 nums = [1, -1, 5, -2, 3], k = 3

返回4， 因为子数组[1, -1, 5, -2]的和等于3，并且是最长的子数组
## 示例 2：
给定 nums = [-2,  -1,  2, 1], k = 3

返回2，因为子数组[2, 1]的和等于3，而且是最长的子数组
## 4. 解决方案
``` c++
int maxSubArrayLen(vector<int>& nums, int k){
    unordered_map<int,int> sums;
    int max_len = 0;
    int sum = 0;
    for(int i = 0; i < nums.size(); i++){
        sum += nums[i];
        if(sum == k){
            max_len = i + 1;
        }else if(sums.count(sum - k)){
            max_len = max(max_len, i - sums[nums[i] - k]);
        }
        if(!sums.count(sum){
            sums[nums[i]] = i;
        }
    }
    return max_len;
}
```
## 5. 思路：
sums保存的值为<cur_sum, index>， 即当前数之和以及当前的索引值，所以如果当前的和等于k，那么就说明前i个数的和为k ,即max length = i + 1;

如果前i个数的和为sum_i, 前j个数的和为sum_j, 并且 sum_i + 8 = sum_j, 那么sum_i = sum_j - 8, 即对于前i个数的和sum_i来说，如果已经存在前j个数的和sum_j = sum_i - 8, 那么i - j 就是所求的max length。
