---
layout: post
title:  根节点到叶节点数字的和
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 129. [Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/description/)
## 2. 描述
给定一颗二叉树，其节点的值为0-9，每一个从根节点到叶节点的路径都表示一个整数，计算所有的这样的路劲表示的整数之和。
## 3. 示例
给定二叉树：
```
    1
   / \
  2   3
```
根节点到叶节点的路径为：1 -> 2，表示整数12

根节点到叶节点的路径为：1 -> 3，表示整数13

因此返回值为： 12 + 13 = 25 
## 4. 解决方案1：
``` c++
int sum = 0;
void SubSum(TreeNode* root, int val){
    if(root == nullptr){
        return;
    }
    int cur_val = val * 10 + root->val;
    if(root->left != nullptr){
        SubSum(root->left, cur_val);
    }
    if(root->right != nullptr){
        SubSum(root->right, cur_val);
    }
    if(root->left == nullptr && root->right == nullptr){
        sum += cur_val;
    }
}
int sumNumbers(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    SubSum(root,0);
    return sum;
}
```