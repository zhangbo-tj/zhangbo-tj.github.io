---
layout: post
title: 砖墙
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 554: [Brick Wall](https://leetcode.com/problems/minimum-index-sum-of-two-lists/description/)
## 2. 描述：
有一面长方形的墙，墙是由很多层砖堆成的，每块砖的高度相同但是宽度不同，需要从墙的顶部到底部画一条垂直的线，并使得穿过的砖最少。

砖墙由一组整数数组表示，每一个数组内的数字都代表砖从左到右的宽度
## 3. 示例：
```
Input: [
[1,2,2,1],
[3,1,2],
[1,3,2],
[2,4],
[3,1,2],
[1,3,1,1]]
Output: 2
```
Explanation: 
![砖墙示意图](/assets/img/HashMap/brick_wall.png)
## 4. 解决方案 :
``` c++
int leastBricks(vector<vector<int>>& wall) {
    int count = 0;
    unordered_map<int,int> wall_map;
    for(auto& row: wall){
        int length = 0;
        for(int i = 0; i < row.size() - 1; i++){
            length += row[i];
            wall_map[length]++;
            count = max(count, wall_map[length]);
        }
    }
    return wall.size() - count;
}
```