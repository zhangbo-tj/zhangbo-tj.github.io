---
layout: post
title:  前K个最常出现的元素
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 347: [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)
## 2. 描述：
给定一个非空数组，计算其前k个最常出现的元素。
## 3. 示例：
给定[1,1,1,2,2,3], k = 2返回[1, 2]
## 4. 解决方案 1：
``` c++
vector<int> topKFrequent(vector<int>& nums, int k) {
    vector<int> res;
    unordered_map<int,int> nums_map;
    for(auto& num: nums){
        nums_map[num]++;
    }
    priority_queue<pair<int,int>> pq;
    for(auto& val: nums_map){
        pq.push(make_pair(val.second,val.first));
    }
    for(int i = 0; i < k; i++){
        res.push_back(pq.top().second);
        pq.pop();
    }
    return res;
}
```
## 5. 解决方案2： 桶排序:
``` c++
vector<int> topKFrequent(vector<int>& nums, int k) {
    vector<int> res;
    unordered_map<int,int> nums_map;
    for(auto& num: nums){
        nums_map[num]++;
    }
    vector<vector<int>> bucket;
    bucket.resize(nums.size() + 1);
    for(auto& num:nums_map){
        bucket[num.second].push_back(num.first);
    }
    for(int i = bucket.size()-1; i>=0; i--){
        for(auto& num: bucket[i]){
            res.push_back(num);
            if(res.size() == k){
                return res;
            }
        }
    }
    return res;
}
```