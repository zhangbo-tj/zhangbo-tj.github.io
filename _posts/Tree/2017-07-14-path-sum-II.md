---
layout: post
title: 记录下和为某值的所有路径
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 113. [Path Sum II](https://leetcode.com/problems/path-sum-ii/description/)
## 2. 描述
给定一个二叉树和一个整数，在所有根节点到叶节点的路径中，找出所经过的节点和等于给定值的所有路径。
## 3. 示例
给定以下二叉树， sum = 22
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7   2   5   1
```
返回值为
```
[
    [5,4,11,2],
    [5,8,4,5]
]
```
## 4. 解决方案：
``` c++
void FindPathSum(TreeNode* root, int sum, vector<int>& path, vector<vector<int>>& paths){
    if(root == nullptr){
        return;
    }
    path.push_back(root->val);
    if(root->left == nullptr && root->right == nullptr && root->val == sum){
        paths.push_back(path);
    }
    FindPathSum(root->left, sum - root->val, path, paths);
    FindPathSum(root->right, sum - root->val, path, paths);
    path.pop_back();
}
vector<vector<int>> pathSum(TreeNode* root, int sum) {
    vector<vector<int>> paths;
    vector<int> path;
    FindPathSum(root, sum, path, paths);
    return paths;
}
```