---
layout: post
title: 二叉树最大路径和
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 124. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)
## 2. 描述
给定一颗二叉树，计算其最大路径和。

路径是指从一个节点到另一个节点的任意路径，要求路径中至少有一个节点而且不一定要经过根节点。
##　3. 示例
1. 输入二叉树：
```
       1
      / \
     2   3
```
2. 输出：6
## 4. 解决方案：
``` c++
int Track(TreeNode* root, int& max_value){
    if(root == nullptr){
        return 0;
    }
    int left_value = max(0, Track(root->left,max_value));
    int right_value = max(0, Track(root->right, max_value));
    max_value = max(max_value, left_value + right_value + root->val);
    return max(left_value, right_value) + root->val;
}
int maxPathSum(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    int max_value = INT_MIN;
    Track(root, max_value);
    return max_value;
}
```