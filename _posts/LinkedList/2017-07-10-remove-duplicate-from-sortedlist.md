---
layout: post
title: 删除单链表内的重复节点
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 83. [Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/)
## 2. 描述:
给定一个有序链表，删除其中的重复元素，使得每个元素都只出现一次。
## 3. 解决方案1：迭代法
``` c++
ListNode* DeleteDuplicates(ListNode* head){
    if(head == nullptr || head->next == nullptr){
        return head;
    }
    ListNode* cur = head;
    while(cur != nullptr){
        while(cur->next != nullptr && cur->next->val == cur->val){
            ListNode* next_node = cur->next;
            cur->next = next_node->next;
            delete next_node;
        }
        cur = cur->next;
    }
    return head;
}
```
## 4. 解决方案2：递归法
``` c++
ListNode* DeleteDuplicates(ListNode* head){
    if(head == nullptr || head->next == nullptr){
        return head;
    }
    head->next = DeleteDuplicates(head->next);
    return head->next->val == head->val ? head->next : head;
}
```
