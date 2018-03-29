---
layout: post
title: 不同的路径
category: 动态规划
tags: Dynamic-Programming LeetCode
Keywords: Dynamic-Programming
description:
---
# Unique Path I
## 1. LeetCode 62. [Unique Paths](https://leetcode.com/problems/unique-paths/description/)
## 2. 描述：
一个机器人位于m * n网格的左上角，它每次只能向右或者向下移动一格。该机器人要想到达网格的右下角，则最多有多少种不同的路径。
## 3. 解题思路：
机器人每次只能向右或者向下移动一格，因此当它到达某个位置时，只有两种可能：
1. 它从该点的上方移动过来（向下移动）
2. 它从该点的左侧移动过来（向右移动）

因此假设机器人到达点(i，j)处有paths[i][j]种路径，则可以得出paths[i][j] = paths[i-1][j] + paths[i][j-1]
![Unique Paths](/assets/img/DP/unique_paths.png)
## 4. 解决方案：
``` c++
int uniquePaths(int m, int n) {
    if(m == 1||n == 1){
        return 1;
    }
    vector<vector<int>> paths(m,vector<int>(n,1));
    for(int i = 1; i < m; i++){
        for(int j = 1; j < n;j++){
            paths[i][j] = paths[i-1][j] + paths[i][j-1];
        }
    }
    return paths[m-1][n-1];
}
```
# Unique Paths II
## 1. LeetCode 63. [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/description/)
## 2. 描述：
在问题“独特的路径”基础之上，在网格中添加一些障碍，用1表示障碍，无法通过；0表示空地，可以通过。重新计算最多有多少种不同的路径可以从左上角走到右下角。
## 3. 示例：
有一个如下的 3 * 3网格：
```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```
该网格中独特的路径个数为 2
## 4. 解题思路
``` c++
int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
    int row = obstacleGrid.size();
    int col = obstacleGrid[0].size();
    vector<vector<int>> paths(row+1, vector<int>(col + 1,0));
    paths[0][1] = 1;
    for(int i = 1; i <= row; i++){
        for(int j = 1; j <= col; j++){
            if(obstacleGrid[i-1][j-1] == 0){
                paths[i][j] = paths[i-1][j] + paths[i][j-1];
            }
        }
    }
    return paths[row][col];
}
```