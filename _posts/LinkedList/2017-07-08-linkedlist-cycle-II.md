---
layout: post
title: 查找链表中环的起始节点
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 142. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)
## 2. 描述：
查找单链表中环的开始位置，如果没有环的话则返回NULL
## 3. Solution
``` c++
ListNode* DetectCycle(ListNode* head){
    if(head == nullptr || head->next == nullptr){
        return nullptr;
    }
    ListNode* slow = head;
    ListNode* fast = head;
    ListNode* entry = head;
    while(fast->next != nullptr && fast->next->next != nullptr){
        slow = slow->next;
        fast = fast->next->next;
        if(slow == fast){
            //当entry和fast相遇时，交点就是环的入口
            while(entry != fast){
                ++entry;
                ++fast;
            }
            return entry;
        }
    }
    return nullptr;
}
```


