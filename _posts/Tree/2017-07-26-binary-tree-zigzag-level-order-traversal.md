---
layout: post
title: 二叉树Zigzag形层级遍历
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 103. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)
## 2. 描述
给定一个二叉树，返回其节点的Zigzag层级遍历的结果（先从左往右，下一层从右往左，然后再交替）。
## 3. 示例
给定二叉树：[3, 9, 20, null, 15, 7]
``` c++
    3
   / \
  9  20
    /  \
   15   7
```
返回结果为：
```
[
  [3],
  [20,9],
  [15,7]
]
```
## 4. 解决方案：
``` c++
void Traversal(TreeNode* root, int depth, vector<vector<int>>& res){
    if(root == nullptr){
        return;
    }
    if(depth == res.size()){
        res.push_back(vector<int>());
    }
    if(depth % 2 == 0){
        res[depth].push_back(root->val);
    }else{
        res[depth].insert(res[depth].begin(), root->val);
    }
    Traversal(root->left, depth + 1, res);
    Traversal(root->right, depth + 1,res);
}
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> res;
    Traversal(root, 0, res);
    return res;
}
```

