---
layout: post
title: 计算二叉树的最小深度
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 111. [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)
## 2. 描述：
给定一个二叉树，计算其最小深度，最小深度是指根节点到叶节点的最短路径。
## 3. 解决方案 1： 递归法
``` c++
int minDepth(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    if(root->left == nullptr) return minDepth(root->right) + 1;
    if(root->right == nullptr) return minDepth(root->left) + 1;
    return min(minDepth(root->left),minDepth(root->right)) + 1;
}
```
##　4. 解决方案 2： BFS
``` c++
int minDepth(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    
    queue<TreeNode*> nodes;
    nodes.push(root);
    int res = 0;
    while(!nodes.empty()){
        res++;
        for(int i = 0, n = nodes.size(); i < n;i++){
            TreeNode* node = nodes.front();
            nodes.pop();
            
            if(node->left != nullptr) nodes.push(node->left);
            if(node->right != nullptr) nodes.push(node->right);
            if(node->left == nullptr && node->right == nullptr) return res;
        }
    }
    return res;
}
```

