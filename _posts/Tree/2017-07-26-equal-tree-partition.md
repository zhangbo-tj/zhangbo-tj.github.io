---
layout: post
title: 等树分割
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 663. [Equal Tree Partition](https://leetcode.com/problems/equal-tree-partition/description/)
## 2. 描述：
给定一颗有n个节点的二叉树，判断能否将二叉树分割为两个子树，并保证分割得到的两个子树的节点和相等。
## 3. 示例：
a. 示例1：
输入:     
```
      5
    /   \
   10   10
       /   \
      2     3
```
输出: True

解释: 
```
    5
   / 
  10
```   
和:15
```
   10
  /  \
 2    3
```
Sum: 15
## 4. 解题思路：
遍历整个树，分别计算所有子树的节点和，如果存在某个子树的节点和等于整颗子树的节点和的一半，则证明存在该子树。
## 5. 解决方案：
``` c++
int SubSum(TreeNode* root, unordered_map<int,int>& sums){
    if(root == nullptr){
        return 0;
    }
    int sum = root->val + SubSum(root->left,sums) + SubSum(root->right, sums);
    sums[sum]++;
    return sum;
}
bool checkEqualTree(TreeNode* root) {
    unordered_map<int,int> sums;
    int sum = SubSum(root, sums);
    if(sum == 0) return sums[sum] > 1;
    return sum % 2 == 0 && sums.count(sum / 2);
}
```
