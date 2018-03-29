---
layout: post
title: 向右旋转链表
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 61. [Rotate List](https://leetcode.com/problems/rotate-list/description/)
## 2. 描述：
给定一个链表，将其向右旋转k个位置，k是非负数
## 3. 示例：
输入：1 -> 2 -> 3 -> 4 -> 5 -> nullptr，k = 2

输出： 4 -> 5 -> 1 -> 2 -> 3 -> nullptr
## 4. 解决方案：
``` c++
ListNode* RotateRight(ListNode* head, int k){
    if(head == nullptr || head->next == nullptr || k == 0){
        return head;
    }
    ListNode* tail = head;
    int len = 1;
    while(tail->next != nullptr){
        tail = tail->next;
        len++;
    }
    tail->next = head;
    for(int i = 0; i < k % len;i++){
        tail = tail->next;
    }
    head = tail->next;
    tail->next = nullptr;
    return head;
}
```
