---
layout: post
title:  根据频率对字符进行排序
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 451: [Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)
## 2. 描述：
给定一个字符串，对其中出现的字符按频率降序排列。
## 3. 示例：
```
Input:
    "tree"
Output:
    "eert"
Explanation:
    'e' appears twice while 'r' and 't' both appear once.
    So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```
## 4. 解决方案 1:
``` c++
string frequencySort(string s) {
    vector<string> bucket(s.size()+ 1,"");
    unordered_map<int,int> counts;
    for(auto& ch:s){
        counts[ch]++;
    }
    for(auto iter: counts){
        bucket[iter.second].append(iter.second,iter.first);
    }
    string result;
    for(int i = bucket.size() - 1; i >= 0; i--){
        result += bucket[i];
    }
    return result;
}
```
## 5. 解决方案 2:
``` c++
string frequencySort(string s) {
    int counts[256] = {0};
    for(auto c: s){
        counts[c]++;
    }
    sort(s.begin(), s.end(), [&](char a,char b){
        return counts[a] > counts[b] ||(counts[a] == counts[b] && a < b);
    });
    return s;
}
```