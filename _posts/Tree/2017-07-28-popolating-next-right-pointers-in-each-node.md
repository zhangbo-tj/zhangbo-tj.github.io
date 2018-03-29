---
layout: post
title: 为节点添加指向其右侧节点的Next指针
category: 二叉树
tags: Tree LeetCode
Keywords: Tree
description:
---
# 问题 I
## 1. LeetCode 116. [Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/description/)
## 2. 描述
给定一个二叉树，其节点定义如下
``` c++
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```
每个节点的next指针初始值为nullptr, 将其设置为在树中的右侧节点。如果其右侧没有节点，则将next指针设为nullptr。

要求使用常量空间，而且假设该树是完全二叉树，即每个节点都有两个子节点
## 3. 示例
a. 给定二叉树：
```
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
```
b. 处理结果为：
```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
```
## 4. 解决方案1：
``` c++
void connect(TreeLinkNode *root) {
    if(root == nullptr){
        return;
    }
    if(root->left != nullptr){
        root->left->next = root->right;
        if(root->next != nullptr){
            root->right->next = root->next->left;
        }
    }
    connect(root->left);
    connect(root->right);
}
```
## 5. 解决方案 2： 
``` c++
void connect(TreeLinkNode *root) {
    if(root == nullptr){
        return;
    }
    
    TreeLinkNode* pre = root;
    TreeLinkNode* cur = nullptr;
    while(pre != nullptr){
        cur = pre;
        while(cur != nullptr && cur->left != nullptr){
            cur->left->next = cur->right;
            if(cur->next != nullptr){
                cur->right->next = cur->next->left;
            }
            cur = cur->next;
        }
        pre = pre->left;
    }
}
```

# 问题 II
## 1. LeetCode 117. Polulating Next Right Pointers in Each Node II
https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/description/
## 2. 描述
与问题“Polulating Next Right Pointers in Each Node”相同，只是要求二叉树可以为任何二叉树
## 3. 示例
a. 输入二叉树
```
         1
       /  \
      2    3
     / \    \
    4   5    7
```
b. 输出结果
```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```
## 4. 解决方案：
``` c++
void connect(TreeLinkNode *root) {
    TreeLinkNode* cur = root;
    TreeLinkNode* head = nullptr;
    TreeLinkNode* prev = nullptr;
    while(cur != nullptr){
        while(cur != nullptr){
            if(cur->left != nullptr){
                if(prev != nullptr){
                    prev->next = cur->left;
                }else{
                    head = cur->left;
                }
                prev = cur->left;
            }
            if(cur->right != nullptr){
                if(prev != nullptr){
                    prev->next = cur->right;
                }else{
                    head = cur->right;
                }
                prev = cur->right;
            }
            cur = cur->next;
        }
        cur = head;
        head = nullptr;
        prev = nullptr;
    }
}
```