---
layout: post
title: 单链表删除节点
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 237. [Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/description/)

## 2. 描述：
删除List中的Node结点
## 3. 解决方案:
``` c++
void DeleteNode(ListNode* node){
    ListNode* temp = node;
    *node = *(node->next);
    delete temp;
}
```
