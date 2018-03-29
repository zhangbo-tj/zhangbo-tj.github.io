---
layout: post
title: 找出两个字符串中的不同字符
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 389: [Find the Difference](https://leetcode.com/problems/find-the-difference/description/)
## 2. 描述：
给定字符串s和t，二者都是由小写字母组成的。其中字符串t是通过将s重新洗牌（shuffling）并添加一个字符后组成的，找出字符串t中添加的字符。
## 3. 示例：
```
Input:  s = "abcd"    t = "abcde"
Output: e
Explanation:
'e' is the letter that was added.
```
## 4. 解决方案 1:
``` c++
char findTheDifference(string s, string t) {
	char char_array[26] = {0};
	for(auto& c:s){
char_array[c - 'a']++;
	}
	for(auto& c: t){
	    char_array[c - 'a']--;
	}
	for(int i = 0; i < 26; i++){
        if(char_array[i] < 0){
	        return 'a'+i;
        }
	}
}
```
## 5. 解决方案 2:
``` c++
char findTheDifference(string s, string t) {
	unordered_map<char,int> chars_map;
	for(auto& c:s){
        chars_map[c]++;
	}
	for(auto& c:t){
        chars_map[c]--;
	}
	for(auto& num:chars_map){
        if(num.second < 0){
	        return num.first;
        }
	}
}
```
## 6. 解决方案 3:
``` c++
char findTheDifference(string s, string t) {
    int res = 0;
    for(auto& c:s){
        res ^= c;
    }
    for(auto& c:t){
        res ^= c;
    }
    return res;
}
```