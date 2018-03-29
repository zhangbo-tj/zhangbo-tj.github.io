---
layout: post
title: 字符串重构
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 205: [Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/description/)
## 2. 描述：
给定两个字符串S和T，判断它们是否是同构的。如果替换S中的字符可以得到T，则二者是同构的。

不会有两个字符同时映射到同一个字符，但是字符可以映射到自身。假定两个字符串是同样长的。
## 3. 示例：
给定 “egg”和"add"，返回true给定“foo”和“bar”，返回false给定   "paper"和"title"，返回true
## 4. 解决方案 1:
``` c++
bool isIsomorphic(string s, string t) {
    unordered_map<char,char> map1;
    unordered_map<char,char> map2;
    for(int i = 0; i < s.size(); i++){
        if((map1[s[i]] && map1[s[i]] != t[i])||
            (map2[t[i]] && map2[t[i]] != s[i])){
        return false;
    }
    map1[s[i]] = t[i];
    map2[t[i]] = s[i];
    }
    return true;
}
```
## 5. 解决方案 2：
``` c++
bool isIsomorphic(string s, string t) {
    unordered_map<char,int> map1;
    unordered_map<char,int> map2;
    unordered_map<char,char> char_map;
    for(int i = 0; i < s.size(); i++){
        map1[s[i]]++;
        map2[t[i]]++;
        if(!char_map[s[i]]) 
            char_map[s[i]] = t[i];
        if(char_map[s[i]] != t[i] || map1[s[i]] != map2[t[i]]){
            return false;
        }
    }
    return true;
}
```
## 6. 解决方案 3:
``` c++
bool isIsomorphic(string s, string t) {
    int m1[256] = {0};
    int m2[256] = {0};
    for(int i = 0; i < s.size(); i++){
        if(m1[s[i]] != m2[t[i]]){return false;}
            m1[s[i]] = i+1;
            m2[t[i]] = i+1;
    }
    return true;
}
```
