---
layout: post
title:  找出所有单词排列组合成的子串
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 30: [Substring with concatenation of all words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/description/)
## 2. 描述：
给定一个字符串和一组长度相同的单词，找出所有单词排列组合后组成的字符串出现的位置
## 3. 示例：
```
Given:
    s: "barfoothefoobarman"
    words: ["foo", "bar"]
You should return the indices: [0,9].
(order does not matter).
```
## 4. 解决方案：
``` c++
vector<int> findSubstring(string s, vector<string>& words) {
    vector<int> res;
    unordered_map<string,int> counts;
    for(auto& str:words){
        counts[str]++;
    }
    int len = words[0].size();
    int num = words.size();
    int n = s.size();
    for(int i = 0; i < n - num * len + 1; i++){
        unordered_map<string,int> cur_counts;
        int j = 0;
        for(; j < num; j++){
        string str = s.substr(i + j * len, len);
        if(counts.find(str) != counts.end()){
            cur_counts[str]++;
            if(cur_counts[str] > counts[str]){
                break;
            }
        }else{
            break;
        }
        }
        if(j == num){
            res.push_back(i);
        }
    }
    return res;
}
```