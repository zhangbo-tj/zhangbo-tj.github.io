---
layout: post
title: 翻转二叉树
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 226. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)
## 2. 描述
给定一颗二叉树，将其翻转
## 3. 解决方案 1：recursive BFS
``` c++
TreeNode* invertTree(TreeNode* root) {
    if(root == nullptr){
        return nullptr;
    }
    
    invertTree(root->left);
    invertTree(root->right);
    swap(root->left, root->right);
    return root;
}
```
## 4. 解决方案 2： BFS
``` c++
TreeNode* invertTree(TreeNode* root) {
    if(root == nullptr){
        return nullptr;
    }
    
    queue<TreeNode*> nodes;
    nodes.push(root);
    
    while(!nodes.empty()){
        TreeNode* node = nodes.front();
        nodes.pop();
        
        if(node->left != nullptr){
            nodes.push(node->left);
        }
        if(node->right != nullptr){
            nodes.push(node->right);
        }
        
        TreeNode* temp = node->left;
        node->left = node->right;
        node->right = temp;
    }
    return root;
}
```