---
layout: post
title: 删除单链表内的重复节点II
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 82. [Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/description/)
## 2. 描述：
给定一个已排序的链表，删除所有有相同值的节点。
## 3. 示例
+ 输入：1 -> 2 -> 3 -> 3 -> 4 -> 4 -> 5
+ 输出：1 -> 2 -> 5

+ 输入： 1  -> 1 -> 1 -> 2 -> 3
+ 输出： 2 -> 3
## 4. 解决方案
``` c++
ListNode* DeleteDuplicates(ListNode* head){
    if(head == nullptr || head->next == nullptr){
        return head;
    }
    ListNode* pre_head = new ListNode(0);
    pre_head->next = head;
    ListNode* pre = pre_head;
    ListNode* cur = head;
    while(cur != nullptr){
        // 如果cur-val == cur->next->val，则删除cur节点，并将cur移动到下一个节点
        while(cur->next != nullptr && cur->next->val == cur->val){
            ListNode* next_node = cur->next;
            delete cur;
            cur = next_node;
        }
        //如果pre->next != cur表明上面的操作中有重复节点，因此需要再删除cur节点
        if(pre->next == cur){
            pre = pre->next;
            cur = cur->next;
        }else{
            ListNode* next_node = cur->next;
            pre->next = cur->next;
            delete cur;
            cur = next_node;
        }
    }
    return pre_head->next;
}
```
