---
layout: post
title: 计算不同二叉搜索树数量
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 96. [Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/description/)
##　2. 描述
给定一整数n, 计算总共存在多少个可以存储1...n的二叉搜索树。
## 3. 示例
n = 3, 则总共有5个完全不相同的二叉搜索树
```
1        3   3   2    1
 \      /   /   / \    \
  3    2   1   1   3    2
 /    /     \            \
2    1      2             3
```
## 4. 解题思路
用DP（n）表示总共可以存在多少个unique BST, 且BST中存在的节点为 1...n

令F(i)表示第i个节点作为根节点的BST的数量，则有 DP（n） = F(1) + F(2) .... + F(n)

假设从1...n中选择一个节点作为根节点，则其左子树的节点为1...i ，右子树的节点为i ... n
因此其左子树可能的数量就为DP(i - 1), 其右子树可能构成的BST数量就为DP(n - i)，则此时颗树中可能构成的BST数量就为DP(i-1) * DP(n - i)

综上，DP（n）
= F（1） + F(2) + F(3) ... + F(n) 

= DP( 0 ) * DP( n - 1) + DP(1) * DP(n - 2)  + DP(2) * DP(n - 3).... + DP(n - 1) * DP(0)
## 5. 解决方案
``` c++
int numTrees(int n) {
    vector<int> nums(n+1, 0);
    nums[0] = 1;
    nums[1] = 1;
    for(int i = 2; i <= n;i++){
        for(int j = 1; j <= i;j++){
            nums[i] += nums[j-1] * nums[i-j];
        }
    }
    return nums[n];
}
```