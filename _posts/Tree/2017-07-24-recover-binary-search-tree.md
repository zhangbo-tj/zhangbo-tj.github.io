---
layout: post
title: 恢复二叉搜索树（BST）
category: 二叉树
tags: Tree LeetCode BST
Keywords: Tree BST
description:
---
## 1. LeetCode 99. [Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/description/)
## 2. 描述
二叉搜书树中的两个节点的值交换了，要求恢复这个BST同时要保持其结构不变，要求空间复杂度为常数。
## 3. 解决方案 1：两次遍历
``` c++
TreeNode* node1 = nullptr;
TreeNode* node2 = nullptr;
TreeNode* pre = nullptr;
TreeNode* next = nullptr;
void Traversal1(TreeNode* root){
    if(root == nullptr) return;
    Traversal1(root->left);
    if(pre != nullptr && pre->val >= root->val){
        node1 = pre;
        return;
    }
    pre = root;
    Traversal1(root->right);
}
void Traversal2(TreeNode* root){
    if(root == nullptr){
        return;
    }
    Traversal2(root->right);
    if(next != nullptr && next->val <= root->val){
        node2 = next;
        return;
    }
    next = root;
    Traversal2(root->left);
}
void recoverTree(TreeNode* root) {
    Traversal1(root);
    Traversal2(root);
    int val = node1->val;
    node1->val = node2->val;
    node2->val = val;
}
```
4. 解决方案 2：一次遍历
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
