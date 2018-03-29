---
layout: post
title:  重复DNA序列
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 187:  [Repeated DNA Sequence](https://leetcode.com/problems/repeated-dna-sequences/description/)
## 2. 描述：
所有的DNA序列都是A, C, G, T四种元组组成的，例如“ACGAATTCCG”，当研究DNA时，有时需要找出其中重复的序列。

写一个函数找出所有10个字符长度的子字符串中，重复出现的子串。
## 3. 示例：
```
Given 
    s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",
Return:
    ["AAAAACCCCC", "CCCCCAAAAA"].
```
## 4. 解决方案：
``` c++
vector<string> findRepeatedDnaSequences(string s) {
    vector<string> res;
    if(s.size() < 10) return res;
    unordered_map<string,int> strs;
    for(int i =0; i < s.length() - 9; i++){
        string temp_str = s.substr(i,10);
        if(strs[temp_str]++ == 1){
            res.push_back(temp_str);
        }
    }
    return res;
}
```