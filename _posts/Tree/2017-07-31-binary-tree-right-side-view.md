---
layout: post
title: 二叉树的右视图
category: 二叉树
tags: Tree LeetCode 
Keywords: Tree
description:
---
## 1. LeetCode 199. [Binary Tree Right Sise View](https://leetcode.com/problems/binary-tree-right-side-view/description/)
## 2. 描述
给定一颗二叉树，想象从右侧观察这个树，返回所看到的节点的值（从上到下）
## 3. 示例
给定二叉树：
```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```
返回结果: [1, 3,4 ]
## 4. 解决方案：
``` c++
vector<int> rightSideView(TreeNode* root) {
    vector<int> res;
    if(root == nullptr){
        return res;
    }
    queue<TreeNode*> nodes;
    nodes.push(root);
    
    while(!nodes.empty()){
        for(int i = 0, len = nodes.size(); i < len;i++){
         TreeNode* node = nodes.front();
            nodes.pop();
            if(node->left != nullptr){
                nodes.push(node->left);
            }
            if(node->right != nullptr){
                nodes.push(node->right);
            }
            if(i == len-1){
                res.push_back(node->val);
            }
        }
    }
    return res;
}
```