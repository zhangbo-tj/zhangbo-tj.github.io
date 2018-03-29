---
layout: post
title: 判断链表中是否存在环
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 141. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/description/)
## 2. 描述：
判断链表中是否存在环
## 3. 解决方案：
``` c++
// Floyd判圈算法
bool HasCycle(ListNode* head){
      if(head == nullptr || head->next == nullptr){
          return nullptr;
    }
    ListNode* slow = head;
    ListNode* fast;
    while(fast->next != nullptr && fast->next->next != nullptr){
        slow = slow->next;
        fast = fast->next->next;
        //如果链表中存在环，则一定会出现slow和fast相同的情况
        if(slow == fast){
            return true;
        }
    }
    return false;
}
```
