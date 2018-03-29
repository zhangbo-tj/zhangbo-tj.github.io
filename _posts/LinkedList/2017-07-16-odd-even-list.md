---
layout: post
title: 奇偶链表
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 328. [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/description/)
## 2. 描述：
给定一个单链表，将所有奇数节点放在偶数节点前面，此处的偶数和奇数是指节点的数量而不是节点的值
## 3. 示例：
输入： 1 -> 2 -> 3 -> 4 -> 5 -> NULL

输出： 1 -> 3 -> 5 -> 2 -> 4 -> NULL
## 4. 解决方案：
``` c++
ListNode* OddEvenList(ListNode* head){
    if(head == nullptr || head->next == nullptr || head->next->next == nullptr){
        return head;
    }
    ListNode* left_head = new ListNode(0);
    ListNode* left_node = left_head;
    ListNode* right_head = new ListNode(0);
    ListNode* right_node = right_head;
    ListNode* cur = head;
    while(cur != nullptr){
        left_node->next = cur;
        left_node = left_node->next;
        if(cur->next != nullptr){
            right_node->next = cur->next;
            right_node = right_node->next;
            cur = cur->next->next;
        }else{
            cur = cur->next;
        }
    }
    left_node->next = right_head->next;
    return left_head->next;
}
```