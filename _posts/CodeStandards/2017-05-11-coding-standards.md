---
layout: post
title: C++编程规范：STL容器
category: C++编程规范
tags: 编程规范
Keywords: C++
description:
---

## 第76条：默认时使用vector，否则选择其他合适的容器
vector具有以下性质：
    * 保证具有所有容器中最低的空间开销（每个对象只需0字节）
    * 保证具有所有容器中对所存放元素进行存取的速度最快
    * 保证具有与身俱来的引用局部性，也就是说容器中相邻对象保证在内存中也相邻，这是其他标准容器都无法保证的
    * 保证具有与C语言兼容的内存布局，这与其他标准容器也不同
## 第77条：用vector和string代替数组
应该使用标准设施代替C风格数组，部分理由如下：
    * 它们能够自动管理内存。不需要比任何合理长度更长的固定缓冲区，㛑不需要胡乱地进行内存重新分配和指针调整
    * 具有丰富的接口，可以实现复杂功能
    * 与C的内存模型兼容
    * 能够提供更大范围的检查，迭代器和索引操作符能够暴露很大范围类型的内存错误
## 第78条：使用vector与非C++API交换数据
1. vector和string::c_str是与非C++API通信的通道，但是不要将迭代器当做指针，要获取vector<T>::iterator iter所引用的元素地址，应该使用&*iter
2. vector的存储区总是连续的，因此访问其第一个元素的地址将返回一个指向其内容的指针，可以使用&v.begin(), &v[0], &v.front()获取v的第一个元素的指针要获取指向Vector第n个元素的指针，应该先做运算再取地址（&v.begin()[n]或&v[n]），而不是先获取头部的指针然后再进行指针运算（（&v.front())[n]），这是因为前一种情况下能够给带检查的标准库实现一个机会，验证没有越界访问v的元素
3. 不要一般性地认为vector的迭代器都是指针，迭代器是完整的类型
4. 虽然大多数标准库实现中string都使用连续内存，但这是没有保证的，所以绝对不要认为获取了string中一个字符地址，它指向的就是连续内存
5. 如果有一个除vector或string之外存放T对象的容器，想将其内容传递给一个非C++  API,而该API需要指向T对象数组的指针，那么应该将容器内容复制到一个vector<T>中，通过后者直接非C++ API通信
## 第79条：在容器中只存储值和智能指针
1. 容器假设它们所存放的是类似值的类型，包括值类型、智能指针和迭代器
2. 异构容器：为了让容器存储和拥有类型不同但是相关类型的对象，应该使用container<shared_ptr<Base>>，另外一种方式是存储代理对象，该对象的非虚拟函数会将调用传递给相应的实际对象的虚拟函数
3. 为了让主容器存放对象，然后在不重排主容器的情况下用不同的排列顺序访问它们，可以设置多个此容器指向主容器，然后用析值比较谓词以不同方式将次容器排序
## 第80条：用push_back代替其他扩展序列的方式
1. 尽可能使用push_back，如果不需要操心插入位置，就应该使用push_back再序列中添加元素
2. push_back时是按照指数级扩大容量的，而不是按固定增量扩大的，因此重新分配和复制的次数将随大小的增长迅速减少。

对于只用push_back调用添加元素的容器而言，无论容器最终大小是多少，每个元素平均只会复制一次。
3. 例外情况：如果知道要添加的范围，即使位于容器的末尾，也应该使用范围插入函数。指数级增长需要分配大量内存，要对此增长微调，可以显式地调用reserve
## 第81条：多用范围操作，少用单元素操作
调用以迭代器范围为参数的构造函数，通常比调用默认构造函数然后再单独插入容器的性能更好。
## 第82条：使用公认的惯用法真正地压缩容量，真正地删除元素
1. cotainer<T>(c).swap(c), shrink_to_fit的惯用法，去除多余容量
container<T>().swap(c),  去除全部内容和容量的惯用法
2. remove算法并不真正地从容器中删除元素，它只是操作于迭代器范围，不调用容器的成员函数，它所做的就是移动值的位置，将不应该“删除”的元素移至范围的开始处，并返回一个迭代器指向最后一个不应该删除元素的下一位置。要真正删除，需要在调用remove之后再调用erase，这就是所谓的“erase-remove”惯用法。
c.erase( remove(c.begin(), c.end(), value), c.end() );
3. 例外情况：通常的的shrink_to_fit惯用法对写时复制方式实现的std::string不适用，总是可行的方法是调用s.reserve(0)，或者是通过编写string( s.begin( ), s.end( ) ).swap(s),用迭代器范围构造函数来压缩string的多余容量。
