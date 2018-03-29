---
layout: post
title: 为二叉树添加一行新节点
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
## 1. LeetCode 623. [Add One Row to Tree](https://leetcode.com/problems/add-one-row-to-tree/description/)
## 2. 描述：
给定一个二叉树的根节点，数值v和深度d, 需要添加一行深度为d，且值为v的节点，根节点的深度为1。添加节点的规则是：给定一个表示深度的正整数d,对每一个深度为d-1的非空节点N，同时创建两个值为v的节点作为N的左子节点和右子节点。而且节点N的原来的左子节点将成为新节点的左子节点，右节点也是这样。如果深度值为1的话，则说明创建一个值为v的节点作为新的根节点，而且原根节点将作为其左子节点。
## 3. 示例1 ：
输入: 
```
      4
    /   \
   2    6
  /  \  / 
 3   1  5 
```
v = 1

d = 2

输出: 
```
      4 
     / \
    1   1
   /     \
  2      6
 / \    / 
3   1  5 
```

## 3. 示例2：
输入：
```
    4
   / 
  2 
 / \ 
3   1 
```
v = 1

d = 3

输出: 
```
         4
        / 
       2
      / \ 
     1   1
    /    \ 
   3      1
```
## 4. 解决方案: 
``` c++
TreeNode* addOneRow(TreeNode* root, int v, int d) {
	if(d == 0 || d == 1){
		TreeNode* node = new TreeNode(v);
		if(d == 1){
			node->left = root;
		}else{
			node->right = root;
		}
		return node;
	}
	if(root && d >= 2){
		root->left = addOneRow(root->left,v, d > 2? d-1:1 );
		root->right = addOneRow(root->right,v,d > 2?d-1:0);
	}
	return root;
}
```
