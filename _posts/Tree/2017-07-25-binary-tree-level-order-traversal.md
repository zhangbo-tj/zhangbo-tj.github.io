---
layout: post
title: 二叉树层级遍历
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 102. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)
## 2. 描述
给定一个二叉树，输出其节点的层级遍历结果
## 3. 示例
```
    3
   / \
  9  20
    /  \
   15   7
```
输出为：
```
[
  [3],
  [9,20],
  [15,7]
]
```
## 4. 解决方案 1：
``` c++
TreeNode* first_node = nullptr;
TreeNode* second_node = nullptr;
TreeNode* prev_node = nullptr;
void Traversal(TreeNode* root){
    if(root == nullptr){
        return;
    }
    
    Traversal(root->left);
    
    if(prev_node != nullptr && first_node == nullptr && prev_node->val >= root->val){
        first_node = prev_node;
    }
    if(prev_node != nullptr && first_node != nullptr && prev_node->val >= root->val){
        second_node = root;
    }
    prev_node = root;
    Traversal(root->right);
}
void recoverTree(TreeNode* root) {
    Traversal(root);
    int val = first_node->val;
    first_node->val = second_node->val;
    second_node->val = val;
}
```
## 5. 解决方案 2：
``` c++
TreeNode* first_node = nullptr;
TreeNode* second_node = nullptr;
TreeNode* prev_node = nullptr;
void Traversal(TreeNode* root){
    if(root == nullptr){
        return;
    }
    
    Traversal(root->left);
    
    if(prev_node != nullptr && first_node == nullptr && prev_node->val >= root->val){
        first_node = prev_node;
    }
    if(prev_node != nullptr && first_node != nullptr && prev_node->val >= root->val){
        second_node = root;
    }
    prev_node = root;
    Traversal(root->right);
}
void recoverTree(TreeNode* root) {
    Traversal(root);
    int val = first_node->val;
    first_node->val = second_node->val;
    second_node->val = val;
}
```
