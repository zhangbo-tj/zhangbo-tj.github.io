---
layout: post
title: C++并发保证
category: CPP11新特性
tags: C++  并发
Keywords: 并发
description:
---
## 1. C++11提供的一般性规范如下：
1. 一般而言，多个线程共享同一个程序库对象而其中至少有一个程序库对象而其中至少一个线程改动对象时，可能会导致不明确行为，除非该类型的对象呗明确指定为“sharable without data races”，或者使用者为它提供的了一个locking机制
2. 某个线程的对象构建或者是析构对象期间，另一个线程使用了该对象时，会导致不明确行为。这些甚至适用于针对线程安全而提供的对象。

## 2. STL容器和容器和容器适配器提供以下保证：
1. 并发的只读访问是允许的。
这意味着调用成员函数begin(), end(), rbegin(), rend(),..等，以及通过迭代器（只要它们不改动容器）都是允许的。
2. 并发处理同一个容器内的不同元素是可以的（除class  vector<bool>外）。
因此不同的线程可以并发地读/写相同容器内的不同元素。

## 3. stream & 并发
对标准stream进行格式化输入和输出的并发处理是可能的，虽然有可能会导致插叙字符（interleaved character）。这些东西默认使用于std::cin,, std::cout,和std::cerr，但是对于string stream, file stream, stream bufffer， 并发处理会导致不明确地行为。

## 4. 其他：
1. exit atexit( )和at_quick_exit( )的并发调用时同步的，相同情况适用于那些设定或取得new,terminate, unspected handler的函数（set_new_handler( ), set_unspected( ), set_terminate( )）
2. 默认分配器的所有成员函数，除了析构函数，其并发处理都被同步（synchronized）

## 5. C++ 11为并发编程提供的改变：
1. 语言核心定义了一个内存模型，保证当更改“被两个不同线程使用”的两个Object时，它们彼此独立，并引入了一个新的关键字thread_local, 用以定义“变量带有thread专属值”。
2. 标准库提供的支持允许启动多线程，包括传递实参、返回数值、跨线程边界传递异常、同步化（synchronize）等，从而使得对“控制流程”和“数据访问”实现同步化。


