---
layout: post
title: 计算二叉树的直径
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 543. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/description/)
## 2. 描述
给定一个二叉树，需要计算该树的直径长度。
二叉树的直径是指树中任意两个节点之间的最长路径，该路径不一定要经过root节点。
## 3. 示例
以下二叉树返回值为3， 最长的路径为[4,2,1,3]和[5,2,1,3]
```
    1
   / \
  2   3
 / \ 
4   5 
```
## 4. 解决方案：
``` c++
int MaxDepth(TreeNode* root, int& diameter){
    if(root == nullptr){
        return 0;
    }
    int left_height = MaxDepth(root->left,diameter);
    int right_height = MaxDepth(root->right,diameter);
    diameter = max(diameter, left_height + right_height);
    return max(left_height,right_height) + 1;
}
int diameterOfBinaryTree(TreeNode* root) {
    int diameter = 0;
    MaxDepth(root, diameter);
    return diameter;
}
```
