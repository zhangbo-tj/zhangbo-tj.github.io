---
layout: post
title: 计算二叉树左叶节点的和
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 404. [Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/description/)
## 2. 描述：
给定一个二叉树，计算其所有左叶节点的和
## 3. 示例
```
  3
 / \
9  20
   / \
  15  7
```
在该二叉树中有两个左叶节点：9和15，所以要返回24
## 4. 解决方案
``` c++
int sumOfLeftLeaves(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    
    int res = 0;
    if(root->left != nullptr && root->left->left == nullptr && root->left->right == nullptr){
        res += root->left->val;
    }else{
        res += sumOfLeftLeaves(root->left);
    }
    if(root->right!= nullptr &&(root->right->left != nullptr || root->right->right != nullptr)){
        res += sumOfLeftLeaves(root->right);
    }
    return res;
}
```
##　5. 解决方案 2：
``` c++
int sumOfLeftLeaves(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    
    if(root->left != nullptr && root->left->left == nullptr && root->left->right == nullptr){
        return root->left->val + sumOfLeftLeaves(root->right);
    }
    return sumOfLeftLeaves(root->left) + sumOfLeftLeaves(root->right);
}
```
