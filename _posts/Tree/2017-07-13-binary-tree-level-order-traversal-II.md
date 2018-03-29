---
layout: post
title: 二叉树层级遍历II
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 107. [Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/)
## 2. 描述
给定一颗二叉树，从下往上每一层的遍历结果（每一层都从左到右）
## 3. 解决方案 1：宽度优先搜索
``` c++
vector<vector<int>> levelOrderBottom(TreeNode* root) {
    vector<vector<int>> res;
    if(root == nullptr){
        return res;
    }
    
    queue<TreeNode*> nodes;
    nodes.push(root);
    
    while(!nodes.empty()){
        vector<int> temp;
        for(int i = 0, n = nodes.size(); i < n; i++){
            TreeNode* node = nodes.front();
            nodes.pop();
            
            temp.push_back(node->val);
            if(node->left != nullptr){
                nodes.push(node->left);
            }
            if(node->right != nullptr){
                nodes.push(node->right);
            }
        }
        res.insert(res.begin(),temp);
    }
    //reverse(res.begin(), res.end());
    return res;
}
```
## 4. 解决方案 2： 
``` c++
int MaxDepth(TreeNode* root){
    if(root == nullptr){
        return 0;
    }
    return max(MaxDepth(root->left), MaxDepth(root->right)) + 1;
}
void LevelOrderBottom(TreeNode* root, vector<vector<int>>& res, int level){
    if(root == nullptr){
        return;
    }
    
    res[level].push_back(root->val);
    LevelOrderBottom(root->left, res, level-1);
    LevelOrderBottom(root->right, res, level-1);
}
vector<vector<int>> levelOrderBottom(TreeNode* root) {
    vector<vector<int>> res;
    if(root == nullptr){
        return res;
    }
    int depth = MaxDepth(root);
    res.resize(depth);
    LevelOrderBottom(root, res, depth-1);
    return res;
}
```
