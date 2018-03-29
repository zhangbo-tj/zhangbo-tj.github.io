---
layout: post
title: 判断一颗树是否为另一颗树的子树
category: 二叉树
tags: Tree LeetCode
Keywords: Tree Subtree
description:
---
## 1. LeetCode 572. [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/description/)
## 2. 描述：
给定非空二叉树 s 和 t ,判断 t 是否为 s 的子树（二者相等的情况也包括在内）
## 3. 解决方案：
``` c++
bool isSame(TreeNode* s, TreeNode* t){
    if(s == nullptr && t == nullptr){
        return true;
    }else if(s != nullptr && t != nullptr &&s->val == t->val){
        return isSame(s->left, t->left) && isSame(s->right, t->right);
    }
    return false;
}
bool isSubtree(TreeNode* s, TreeNode* t) {
    if(t == nullptr){
        return true;
    }else if(s == nullptr && t != nullptr){
     return false; 
    }
    else if(t->val == s->val && isSame(s,t)){
        return true;
    }
    return isSubtree(s->left, t) || isSubtree(s->right, t);
}
```