---
layout: post
title: 成对交换链表中的相邻节点
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 24. [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)
## 2. 描述：
给定一个链表，依次交换相邻的两个节点
## 3. 示例：
输入： 1 -> 2 -> 3 -> 4

输出： 2 -> 1 -> 4 -> 3
## 4. 解决方案1：迭代法
``` c++
ListNode* SwapPairs(ListNode* head){
    if(head == nullptr || head->next == nullptr){
        return head;
    }
    ListNode* pre_head = new ListNode(0);
    pre_head->next = head;
    ListNode* tail = pre_head;
    ListNode* cur = head;
    while(cur != nullptr && cur->next != nullptr){
        ListNode* next_node = cur->next;
        ListNode* last_node = cur->next->next;
        cur->next = cur->next->next;
        next_node->next = cur;
        tail->next = next_node;
        tail = cur;
        cur = cur->next;
    }
    return pre_head->next;
}
```
## 5. 解决方案2：递归法
``` c++
ListNode* SwapPairs(ListNode* head){
    if(head == nullptr || head->next == nullptr){
        return head;
    }
    ListNode* next_node = head->next;
    head->next = SwapPairs(head->next->next);
    next_node->next = head;
    return next_node;
}
```
