---
layout: post
title: 验证二叉树是否为二叉搜索树（BST）
category: 二叉树
tags: Tree LeetCode BST
Keywords: Tree BST
description:
---
## 1. LeetCode 98. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
## 2. 描述
给定一颗二叉树，判断该二叉树是否为二叉搜索树。
## 3. 解决方案 1：
``` c++
bool LargerThanPre(TreeNode* root){
    TreeNode* pre = root->left;
    while(pre->right != nullptr){
        pre = pre->right;
    }
    return pre->val < root->val;
}
bool SmallerThanNext(TreeNode* root){
    TreeNode* next = root->right;
    while(next->left != nullptr){
        next = next->left;
    }
    return next->val > root->val;
}
bool isValidBST(TreeNode* root) {
    if(root == nullptr || (root->left == nullptr && root->right == nullptr)){
        return true;
    }
    
    bool left_valid = (root->left == nullptr) ||
        (root->left != nullptr && root->left->val < root->val && LargerThanPre(root) && isValidBST(root->left));
    
    if(left_valid == true){
        return root->right == nullptr ||
            (root->right != nullptr && root->right->val > root->val && SmallerThanNext(root) && isValidBST(root->right));
    }
    
    return false;
}
```
## 4. 解决方案 2：中序遍历
``` c++
bool Validate(TreeNode* root, TreeNode* &prev){
    if(root == nullptr){return true;}
    if(!Validate(root->left,prev)){
        return false;
    }
    
    if(prev != nullptr && prev->val >= root->val){
        return false;
    }
    prev = root;
    return Validate(root->right, prev);
}
bool isValidBST(TreeNode* root) {
    TreeNode* prev = nullptr;
    return Validate(root,prev);
}
```