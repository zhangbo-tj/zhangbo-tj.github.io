---
layout: post
title: 计算能组成的最长回文
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 409: [Longest Parlindrome](https://leetcode.com/problems/longest-palindrome/description/)
## 2. 描述：
给定一个包含大小写字母的字符串，计算使用该字符串中的所有字母能组成的最长回文的长度

字母是大小写敏感的，“Aa”不是一个有效的回文
## 3. 示例：
```
Input:   "abccccdd"
Output：  7
Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```
## 4. 解决方案:
``` c++
class Solution {
public:
    int longestPalindrome(string s) {
        if(s.size() <= 1) return s.size();
        unordered_map<char,int> chars;
        for(auto& c:s){
            chars[c]++;
        }
        int res = 0;
        for(auto& num:chars){
            res += (num.second / 2) * 2;
        }
        return (res == s.size())?res:res + 1;
    }
};
```
