---
layout: post
title:  对字谜进行分组
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 49: [Group Anagrams](https://leetcode.com/problems/group-anagrams/description/)
## 2. 描述：
给定一组字符串，将字母数量相同的字符串进行分组
## 3. 示例：
```
given: 
    ["eat", "tea", "tan", "ate", "nat", "bat"], 
Return:
    [
        ["ate", "eat","tea"],
        ["nat","tan"],
        ["bat"]
    ]
```
## 4. 解决方案：
``` c++
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    vector<vector<string>> result;
    unordered_map<string,int> str_map;
    for(auto& str:strs){
        string temp_str(str);
        sort(temp_str.begin(),temp_str.end(),[](char a, char b){return a < b;});
        if(str_map.find(temp_str) != str_map.end()){
            int index = str_map[temp_str];
            result[index].push_back(str);
        }else{
            str_map[temp_str] = result.size();
            result.push_back(vector<string>());
            result.back().push_back(str);
        }
    }
    return result;
}
```