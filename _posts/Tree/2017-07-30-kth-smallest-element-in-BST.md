---
layout: post
title:  BST中的第K个最小元素
category: 二叉树
tags: Tree LeetCode BST
Keywords: Tree BST
description:
---
## 1. LeetCode 230. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)
## 2. 描述：
给定一颗二叉搜索树，找出其第K个最小的元素
## 3. 解决方案：
``` c++
int Find(TreeNode* root, int& k){
    if(root){
        int x = Find(root->left, k);
        return !k ? x: !--k ? root->val : Find(root->right, k);
    }else{
        return 0;
    }
}
int kthSmallest(TreeNode* root, int k) {
    return Find(root,k);
}
```