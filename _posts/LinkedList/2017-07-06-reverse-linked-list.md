---
layout: post
title: 翻转单链表
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 206. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)
## 2. 描述：
反转单链表
## 3. 迭代法：
时间复杂度O(n), 空间复杂度O(1)原理就是将node的next节点指向它之前的节点，由于单链表中不存储previous节点，所以要提前存储该节点，同时还要存储curNode的next节点。
``` c++
public ListNode* ReverseList(ListNode* head){
    ListNode* pre = nullptr;
    ListNode* cur = head;
    while(cur != nullptr){
        ListNode* next = cur->next;
        cur->next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}
```
## 4. 递归法：时间复杂度O(n), 空间复杂度O(n)
``` c++
ListNode* ReverseList(ListNode* head){
    if(head==nullptr ||head->next == nullptr){
        return head;
    }
    ListNode* node = ReverseList(head->next);
    head->next->next = head;
    head->next = nullptr;
    return node;
}
```
	