---
layout: post
title: 翻转指定单链表指定范围内的节点
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 92. [Reverse Linked ListII](https://leetcode.com/problems/reverse-linked-list-ii/description/)
## 2. 描述：
反转一个链表中[m, n]范围内的节点，要求只遍历一次节点而且不占用额外空间。
## 3. 解决方案：
``` c++
ListNode* ReverseList(ListNode* head,int m,int n){
    if(head == nullptr || m==n){
        return head;
    }
    n -= m;
    ListNode* pre_head = new ListNode(0);
    pre_head->next = head;
    ListNode* pre = pre_head;
    while(--m){
        pre = pre->next;
    }
    ListNode* cur = pre->next;
    //将next_node插入到pre和cur中间，形成pre->next_node->cur
    while(n--){
        ListNode* next_node = cur->next;
        cur->next = next_node->next;
        next_node->next = pre->next;
        pre->next = next_node;
    }
    ListNode* new_head = pre_head->next;
    delete pre_head;
    pre_head = nullptr;
    return new_head;
}
```
