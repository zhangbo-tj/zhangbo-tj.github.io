---
layout: post
title:  找出系统中的重复文件
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 609: [Find Duplicate File in System](https://leetcode.com/problems/find-duplicate-file-in-system/description/)
## 2. 描述：
给定一组路径信息，以及该路径下所有文件及其内容，需要找出所有内容重复的文件的路径信息。

一组重复的文件路径中，至少有两个文件的内容是相同的。

一个简单的路径字符串的格式为：

"root/d1/d2/...dm/  f1.txt(f1_content), f2.txt(f2_content), ....fn.txt(fn_content)";

其含义是在路径root/d1/d2/.../dm下存在n个文件，其内容分别为f1_content, f2_content, ....fn_content。

输出：一组重复文件的路径。
## 3. 示例：
``` 
Input:
    ["root/a 1.txt(abcd) 2.txt(efgh)", "root/c 3.txt(abcd)", "root/c/d 4.txt(efgh)", "root 4.txt(efgh)"]
Output: 
    [["root/a/2.txt","root/c/d/4.txt","root/4.txt"],["root/a/1.txt","root/c/3.txt"]]
```
## 4. 解决方案：
``` c++
vector<vector<string>> findDuplicate(vector<string>& paths) {
    vector<vector<string>> res;
    unordered_map<string,vector<string>> files_map;
    for(auto& path: paths){
        stringstream ss(path);
        string root;
        getline(ss, root,' ');
        string str;
        while(getline(ss,str,' ')){
            string file_name = root + "/"+str.substr(0,str.find('('));
            string content = str.substr(str.find('('), str.find(')'));
            files_map[content].push_back(file_name);
        }
    }
    for(auto& file: files_map){
        if(file.second.size() > 1){
            res.push_back(file.second);
        }
    }
    return res;
}
```
## 5. 扩展问题：
1. 在真实的文件系统中，使用DFS还是BFS搜索文件？一般来说，BFS使用的内存比DFS要多，但是BFS可以利用文件在目录中的位置，因此速度也会更快一些。
2. 如果文件非常大（GB level）,那应该怎样修改程序？在真实的文件系统中，我们并不对整个文件的内容做hash, 而是首先根据文件大小对所有的文件进行分组，尺寸不同的文件其内容一定不会是相同的。然后对大小相同的文件进行Hash（比如使用MD5）,只有md5的结果相同的情况下，才对文件进行一个字节一个字节地比对。
3. 如果一次只能读取1KB的文件内容，那么应该怎样修改程序？先对1KB的内容进行Hash，如果hash的结果相同的话才进行一个字节一个字节地比较
