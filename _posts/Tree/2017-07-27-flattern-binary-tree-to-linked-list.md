---
layout: post
title: 将二叉树转换为链表
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 114. [Flattern Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/description/)
## 2. 描述
给定一颗二叉树，将其转换为链表（in-place）
## 3. 解决方案 1：递归法
``` c++
TreeNode* PreOrder(TreeNode* root){
    if(root == nullptr){
        return nullptr;
    }
    
    TreeNode* left_node = PreOrder(root->left);
    TreeNode* right_node = PreOrder(root->right);
    
    TreeNode* cur = root;
    cur->right = left_node;
    cur->left = nullptr;
    while(cur->right != nullptr){
        cur = cur->right;
    }
    cur->right = right_node;
    cur->left = nullptr;
    return root;
}
void flatten(TreeNode* root) {
    PreOrder(root);
}
```
4. 解决方案 2：非迭代法
``` c++
TreeNode* PreOrder(TreeNode* root){
    if(root == nullptr){
        return nullptr;
    }
    
    TreeNode* left_node = PreOrder(root->left);
    TreeNode* right_node = PreOrder(root->right);
    
    TreeNode* cur = root;
    cur->right = left_node;
    cur->left = nullptr;
    while(cur->right != nullptr){
        cur = cur->right;
    }
    cur->right = right_node;
    cur->left = nullptr;
    return root;
}
void flatten(TreeNode* root) {
    PreOrder(root);
}
```
