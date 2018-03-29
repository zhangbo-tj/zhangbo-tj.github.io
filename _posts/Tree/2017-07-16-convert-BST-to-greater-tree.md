---
layout: post
title: 将二叉查找树转换为大树
category: 二叉树
tags: Tree LeetCode BST
Keywords: Tree BST Greater-Tree
description:
---
## 1. LeetCode 538. [Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)
## 2. 描述
给定一个二叉搜索树（BST）,将其转换为Greater Tree, 要求BST中每个节点的值都等于原值加上所有大于其原值的节点。
## 3. 示例
输入: The root of a Binary Search Tree like this:
```
   5
  / \
 2   13
```
输出: The root of a Greater Tree like this:
```
  18
 / \
20  13
```
## 4. 解题思路
二叉树中序遍历的改良版，可以修改为右子节点 -> 父节点  -> 左子节点
## 5. 解决方案
``` c++
int cur_sum = 0;
void Travel(TreeNode* root){
    if(root == nullptr){
        return;
    }
    
    if(root->right != nullptr){
        Travel(root->right);
    }
    root->val += cur_sum;
    cur_sum = root->val;
    if(root->left != nullptr){
        Travel(root->left);
    }
}
TreeNode* convertBST(TreeNode* root) {
    Travel(root);
    return root;
}
```
