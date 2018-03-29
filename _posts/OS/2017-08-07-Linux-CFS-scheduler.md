---
layout: post
title: Linux CFS 调度算法
category: 操作系统
tags: OS Scheduler
Keywords: Sheduler
description:
---

## 1. 算法特点
1. CFS(Completely Fair Scheduler) - 完全公平的调度算法
2. 试图按照对CPU时间的最大需要运行任务，这有助于确保每个进程可以获得对CPU的公平共享
3. 使用概念virtual runtime表示该进程已经在CPU上运行的时间，virtual runtime值越大，则表明该进程已经运行了较长时间，因此该进程被再次调度的可能性就越小

## 2. CFS数据结构
使用按virual runtime排序的红黑树(Red-Black tree), 其特点如下：
+ 红黑树可以始终保持平衡
+ 查找复杂度为O(lgN),而获取最左侧节点(virtual runtime值最小)的复杂度为O(1)
+ 添加节点的复杂度为O(lgN)

![Linux CFS数据结构](/assets/img/OS/LinuxCFS.png)

## 3. 调度过程
1. 每次调度程序都选择红黑树最左侧的节点运行(其virtual runtime最小)，将其从红黑树中删除，并调整红黑树
2. 每隔一段时间，就更新正在运行的进程的virtual runtime，并将更新后的值与当前红黑树中最左侧节点的virtual runtime相比较
    + 如果小于最左侧节点的virtual runtime，则继续执行当前进程
    + 如果大于最左侧节点的virtual runtime，则抢占式地运行最左侧的节点，并将当前正在运行的进程重新放到红黑树中
3. 进程的virtual runtime增长速度取决于其优先级
    + 优先级越低，则virtual runtime增长越快，因而被再次调用的可能性就越小
    + 优先级越高，则virtual runtime增长越慢，因而被再次调用的可能性就越大

## 4. 性能
1. 选取下一个任务：O(1)
2. 将新任务添加到任务队列中：O(lgN)