---
layout: post
title: 移除链表中的倒数第N个数
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 19. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)
## 2. 描述：
给定一个链表，移除其从末尾数第n个节点
## 3. 示例：
输入： 1 -> 2 -> 3 -> 4 -> 5，n = 2
输出： 1 -> 2 -> 3 -> 5
## 4. 解决方案：
``` c++
ListNode* RemoveNthFromEnd(ListNode* head, int n){
    if(head == nullptr|| n == 0){
        return head;
    }
    ListNode* pre_head = new ListNode(0);
    pre_head->next = head;
    ListNode* cur = pre_head;
    for(int i = 0; i < n + 1;i++){
        cur = cur->next;
    }
    ListNode* pre = pre_head;
    while(cur != nullptr){
        cur = cur->next;
        pre = pre->next;
    }
    ListNode* deleted_node = pre->next;
    pre->next = pre->next->next;
    delete deleted_node;
    head = pre_head->next;
    delete pre_head;
    return pre_head->next;
}
```