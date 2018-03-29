---
layout: post
title: 将有序数组转换为二叉查找树
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 108. [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)
## 2. 描述：
给定一个递增的数组，将其转换为平衡的BST(二叉搜索树)
## 3. 解决方案 1：递归法
``` c++
TreeNode* ArrayToBST(vector<int>& nums, int left, int right){
    if(left > right){
        return nullptr;
    }
    
    int mid = (left + right) / 2;
    TreeNode* root = new TreeNode(nums[mid]);
    root->left = ArrayToBST(nums, left, mid-1);
    root->right = ArrayToBST(nums, mid + 1, right);
    return root;
}
TreeNode* sortedArrayToBST(vector<int>& nums) {
    if(nums.empty()){
        return nullptr;
    }
    return ArrayToBST(nums, 0, nums.size()-1);
}
```
## 4. 解决方案 2：迭代法
``` c++
TreeNode* sortedArrayToBST(vector<int>& nums) {
    if(nums.empty()){
        return nullptr;
    }
    
    TreeNode* root = new TreeNode(0);
    stack<TreeNode*> node_stack;
    node_stack.push(root);
    stack<int> left_stack;
    left_stack.push(0);
    stack<int> right_stack;
    right_stack.push(nums.size() - 1);
    
    while(!node_stack.empty()){
        TreeNode* node = node_stack.top();
        node_stack.pop();
        
        int left_index = left_stack.top();
        left_stack.pop();
        int right_index = right_stack.top();
        right_stack.pop();
        
        int mid_index = (left_index + right_index)/2;
        node->val = nums[mid_index];
        if(left_index <= mid_index - 1){
            node->left = new TreeNode(0);
            node_stack.push(node->left);
            left_stack.push(left_index);
            right_stack.push(mid_index - 1);
        }
        if(mid_index + 1 <= right_index){
            node->right = new TreeNode(0);
            node_stack.push(node->right);
            left_stack.push(mid_index + 1);
            right_stack.push(right_index);
        }
    }
    return root;
}
```