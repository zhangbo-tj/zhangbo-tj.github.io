---
layout: post
title: 计算二叉树的倾斜值
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 563. [Binary Tree Tilt](https://leetcode.com/problems/binary-tree-tilt/description/)
## 2. 描述
给定一个二叉树，返回整个树的tilt(倾斜值)
一个树节点的tilt的计算方法为：其左子树节点的和和右子树节点的和之间差的绝对值，NULL节点的tilt值为0。
## 3. 示例
输入: 
```
    1
   / \
  2   3
```
输出: 1

解释:

* Tilt of node 2 : 0
* Tilt of node 3 : 0
* Tilt of node 1 : |2-3| = 1
* Tilt of binary tree : 0 + 0 + 1 = 1
## 4. 解决方案 1：
``` c++
pair<int,int> FindSumTilt(TreeNode* root){
    if(root == nullptr){
        return make_pair(0,0);
    }
    
    auto left_res = FindSumTilt(root->left);
    auto right_res = FindSumTilt(root->right);
    
    int sum = left_res.first + right_res.first + root->val;
    int tilt = left_res.second + right_res.second + abs(left_res.first - right_res.first);
    return make_pair(sum,tilt);
}
int findTilt(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    auto res = FindSumTilt(root);
    return res.second;
}
```
## 5. 解决方案 2： 
``` c++
int tilt = 0; 
int AddUp(TreeNode* root){
    if(root == nullptr){
        return 0;
    }
    
    int left = AddUp(root->left);
    int right = AddUp(root->right);
    tilt += abs(left - right);
    return left + right + root->val;
}
int findTilt(TreeNode* root) {
    AddUp(root);
    return tilt;
}
```