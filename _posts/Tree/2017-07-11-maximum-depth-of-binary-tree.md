---
layout: post
title: 计算二叉树的最大深度
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 104. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
## 2. 描述：
给定一个二叉树，计算其最大深度。
## 3. 解决方案 1：深度优先搜索
``` c++
int maxDepth(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    return max(maxDepth(root->left),maxDepth(root->right)) + 1;
}
```
## 4. 解决方案 2：
``` c++
int maxDepth(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    
    queue<TreeNode*> nodes;
    int res = 0;
    nodes.push(root);
    
    while(!nodes.empty()){
        res++;
        //此处不能直接用 i < nodes.size(),因为nodes.size()会增大
        for(int i = 0, n = nodes.size(); i < n; i++){
            TreeNode* node = nodes.front();
            nodes.pop();
            
            if(node->left != nullptr){
                nodes.push(node->left);
            }
            if(node->right != nullptr){
                nodes.push(node->right);
            }
        }
    }
    return res;
}
```