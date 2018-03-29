---
layout: post
title: 计算两个单链表相交位置的起点
category: 链表
tags: LinkedList LeetCode
Keywords: LinkedList LeetCode
description:
---
## 1. LeetCode 160. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)
## 2. 描述：
计算两个链表的相交部分的起始节点
## 3. 解决方案：
``` c++
ListNode* GetIntersectionNode(ListNode* headA,ListNode* headB){
    if(headA == nullptr || headB == nullptr){
        return nullptr;
    }
    ListNode* p1 = headA;
    ListNode* p2 = headB;
    while(p1 != nullptr && p2 != nullptr && p1 != p2){
        p1 = p1->next;
        p2 = p2->next;
        //如果两个链表有交点，则此时二者位于相交节点
        //如果两个链表没有交点，则此时二者处于链表末尾的nullptr
        if(p1 == p2) return p1;
        // 如果其中一个指针先到达了链表尾部，则将其移动到另一个链表的头部，
        // 这样可以保证两个指针走过的路径是同样长的
        if(p1 == nullptr) p1 = headB;
        if(p2 == nullptr) p2 = headA;
    }
    return p1;
}
```
	