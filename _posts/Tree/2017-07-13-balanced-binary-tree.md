---
layout: post
title: 判断二叉树是否是平衡二叉树
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 110. [Balanced Tree](https://leetcode.com/problems/balanced-binary-tree/description/)
## 2. 描述
给定一个二叉树，判断它是否是平衡二叉树（左右两个子节点的高度差不超过1）
## 3. 解决方案 1：
``` c++
int Depth(TreeNode* root){
    if(root == nullptr){
        return 0;
    }
    return max(Depth(root->left), Depth(root->right)) + 1;
}
bool isBalanced(TreeNode* root) {
    if(root == nullptr){
        return true;
    }
    
    int left_height = Depth(root->left);
    int right_height = Depth(root->right);
    return abs(left_height - right_height) <= 1 && 
        isBalanced(root->left) && 
        isBalanced(root->right);
}
```
## 4. 解决方案 2：
``` c++
int DFSHeight(TreeNode* root){
    if(root == nullptr){
        return 0;
    }
    
    int left_height = DFSHeight(root->left);
    if(left_height == -1) return -1;
    
    int right_height = DFSHeight(root->right);
    if(right_height == -1) return -1;
    
    if(abs(left_height - right_height) > 1){
        return -1;
    }
    return max(left_height,right_height)+1;
}
bool isBalanced(TreeNode* root) {
    return DFSHeight(root) != -1;
}
```
