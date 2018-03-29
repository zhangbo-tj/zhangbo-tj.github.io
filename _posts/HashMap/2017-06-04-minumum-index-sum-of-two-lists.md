---
layout: post
title: 计算两个字符串数组的最小索引和
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 599:  [Minimum Index Sum of Two Lists](https://leetcode.com/problems/minimum-index-sum-of-two-lists/description/)
## 2. 描述：
假设两个人要选一家餐馆吃饭，而且他们两个人都有一组喜欢的餐馆。

需要找出两人都喜欢的餐馆中索引和最小的餐馆，如果有多个这样的结果，则把它们都输出。假定一定至少存在一个结果。
## 3. 示例：
```
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
Output: ["Shogun"]
Explanation: The only restaurant they both like is "Shogun".
```
## 4. 解决方案 1:
``` c++
vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
    map<int,vector<string>> rank;
    for(int i = 0; i < list2.size(); i++){
        auto offset = find(list1.begin(), list1.end(),list2[i]);
        if(offset != list1.end()){
            int sum = offset - list1.begin() + i;
            rank[sum].push_back(list2[i]);
        }
    }
    return rank.begin()->second;
}
```
## 5. 解决方案 2:
``` c++
vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
    vector<string> result;
    int index_sum = INT_MAX;
    for(int i = 0; i < list2.size(); i++){
        auto offset = find(list1.begin(), list1.end(), list2[i]);
        if(offset != list1.end()){
            int sum = offset - list1.begin() + i;
            if(sum < index_sum){
                result.clear();
                result.push_back(list2[i]);
                index_sum = sum;
            }else if(sum == index_sum){
                result.push_back(list2[i]);
            }
        }
    }
    return result;
}
```
