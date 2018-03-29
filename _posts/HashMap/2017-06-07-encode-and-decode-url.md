---
layout: post
title: URL加密和解析
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 535: [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/description/)
## 2. 描述：
TinyURL是当你输入一个类似“https:/ /leetcode.com/problems/design-tinyurl“的URL时，会自动返回一个更短一些的URL，例如"http://tinyurl.com /4e9iAk"

设计一个对URL进行编码和解码的方法，不限制编码和解码的算法，只需要二者之间是互逆的。
## 3. 解决方案 1:
``` c++
class Solution {
public:
    unordered_map<string,string> long2short;
    unordered_map<string,string> short2long;
    string base_url = "http: //tinyurl.com/";
    string chars = "abcdefghijklmnopqrstuvwxyz1234567890";
    
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        if(long2short.find(longUrl)!= long2short.end()) return long2short[longUrl];
        string short_url;
        do{
            for(int i = 0; i < 6; i++){
                int index = rand() % chars.size();
                short_url[i] = chars[index]; 
            }
        }while(short2long.find(short_url) != short2long.end());
        long2short[longUrl] = short_url;
        short2long[short_url] = longUrl;
        return base_url + short_url;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
    shortUrl.replace(0,base_url.size(),"");
    return short2long[shortUrl];
    }
};
// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```