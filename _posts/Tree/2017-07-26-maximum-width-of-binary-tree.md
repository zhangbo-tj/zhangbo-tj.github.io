---
layout: post
title: 二叉树的最大宽度
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 662. [Maximum Width of Binary Tree](https://leetcode.com/contest/leetcode-weekly-contest-46/problems/maximum-width-of-binary-tree/)
## 2. 描述
给定一颗二叉树，计算该二叉树的最大宽度。二叉树的宽度是指所有层的最大宽度。
二叉树每一层的宽度是指最左侧和最右侧的节点之间的距离，而二者之间的nullptr节点也被包含在内。
## 3. 示例
### 示例1：
输入
```
              1
         /   \
        3     2
       / \       \  
      5   3       9 
```
输出: 4

解释：该树的最大宽度位于第三层(5, 3, null, 9), 长度为4

### 示例2：
输入: 
```
          1
         /  
        3    
       /  \       
      5     3     
```
输出: 2

解释：该二叉树的最大宽度位于第三层（5，3），长度为2

## 4. 解题思路
分别记录每层最左侧的节点和最右侧节点的索引，然后使用DFS遍历所有的节点，在该过程中需要对所有的节点进行编号，左节点编号为 2 * i, 右子节点值为 2 * i + 1。
## 5. 解决方案:
``` c++
void WidthOfBinaryTree(TreeNode* root, int depth,int order, vector<pair<int,int>>& ends){
    if(root == nullptr){
        return;
    }
    if(depth == ends.size()){
        ends.push_back(make_pair(order,order));
    }
    ends[depth].first = min(order,ends[depth].first);
    ends[depth].second = max(order,ends[depth].second);
    
    WidthOfBinaryTree(root->left, depth + 1, 2* order, ends);
    WidthOfBinaryTree(root->right, depth + 1, 2 * order +1 , ends);
}
int widthOfBinaryTree(TreeNode* root) {
    if(root == nullptr){
        return 0;
    }
    vector<pair<int,int>> ends;
    WidthOfBinaryTree(root,0,1,ends);
    int max_len = 0;
    for(auto end: ends){
        int len = end.second - end.first + 1;
        max_len = max(len, max_len);
    }
    return max_len;
}
```