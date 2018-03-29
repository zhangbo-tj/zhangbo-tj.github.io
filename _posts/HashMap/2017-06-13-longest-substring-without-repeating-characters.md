---
layout: post
title:  最长的无重复子串
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 3: [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)
## 2. 描述：
给定一个字符串，找出没有重复字符的最长子串
## 3. 示例1：
给定“abcabcbb”，结果为“abc”，返回3；给定“bbbb”，结果为“b”，返回值为1;

给定“pwwkew”,结果为“wke”,返回值为3； 注意结果必须为子字符串。
## 4. 解决方案:
``` c++
int lengthOfLongestSubstring(string s) {
    int max_len = 0;
    unordered_map<char,int> chars;
    for(int left = 0, right = 0; right < s.length(); right++){
        //如果遇到已经出现过的字符，则更新左指针
        if(chars.find(s[right]) != chars.end()){
            left = max(left, chars[s[right]] + 1);
        }
        chars[s[right]] = right;
        max_len = max(max_len, right-left + 1);
    }
    return max_len;
}
```