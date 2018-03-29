---
layout: post
title: 找出二叉查找树中的重复节点
category: 二叉树
tags: Tree LeetCode BST
Keywords: Tree BST Mode
description:
---
## 1. LeetCode 501. [Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)
## 2. 描述
给定一个有重复值的二叉搜索树，找出该树中所有出现最频繁的元素。如果有多个这样的元素，则这些元素的输出顺序可以是任意的。
## 3. 示例：
给定BST: [1, null, 2, 2]，返回值为2
```
  1
   \
    2
   /
  2
```
## 4. 解题思路
对该二叉搜索树进行中序遍历就会得到一个有序数组，根据有序数组中的连续出现的数字就可以找出所求的元素。
中序遍历的复杂度为O(N)，但是要达到O(1)复杂度的话，就不能存储节点的值，因此可以采用两次遍历二叉搜索树的方式：
1. 第一次遍历找出该树中的元素最多重复多少次
2. 第二次遍历根据最高重复次数找出所有的符合要求的元素
参考自[LeetCode Solution](https://discuss.leetcode.com/topic/77372/11-liner-c-o-n-time-o-1-extra-space-in-order-traversal-detailed-explanation)
## 5. 解决方案
``` c++
vector<int> modes;
int cur_val = INT_MIN;
int cur_count = 0;
int max_count = 0;
int modes_count = 0;
void HandleNode(int val){
    if(cur_val != val){
        cur_val = val;
        cur_count = 0;
    }
    cur_count++;
    if(cur_count > max_count){
        max_count = cur_count;
        modes_count = 1;
    }else if(cur_count == max_count){
        if(!modes.empty()){
            modes[modes_count] = cur_val;
        }
        modes_count++;
    }
}
void InorderTree(TreeNode* root){
    if(root == nullptr){
        return;
    }
    InorderTree(root->left);
    HandleNode(root->val);
    InorderTree(root->right);
}
vector<int> findMode(TreeNode* root) {
    modes.clear();
    InorderTree(root);
    modes.resize(modes_count);
    modes_count = 0;
    cur_count = 0;
    cur_val = INT_MIN;
    InorderTree(root);
    return modes;
}
```