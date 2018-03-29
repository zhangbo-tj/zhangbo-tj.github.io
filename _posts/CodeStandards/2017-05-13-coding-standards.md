---
layout: post
title: C++编程规范：类型安全
category: C++编程规范
tags: 编程规范
Keywords: C++
description:
---
## 第90条：避免使用类型分支，多使用多态
1. 避免通过对象类型分支来定制行为。使用模板和虚函数，让类型自己来决定行为
## 第91条：依赖类型，而非其表示方式
2. C++标准对类型的内存表示方式只有如下几个规定：
    * 整数保证以2为基数
    * 负整数保证以2的补码形式表示
    * 普通旧式数据类型的内存布局与C兼容，成员变量根据声明的顺序存储
    * int至少有16位
3. 但是一下说法并不保证成立：
    * 指针和int的大小并不总是相同的，而且并不总是能任意互相转换的
    * 类的布局并不总是根据声明的顺序布局基类的和成员
## 第92条：避免使用reinterpret_cast
1. reinterpret_cast表示的是程序员对对象表示方式的最强假设，其后果实际上取决于具体编译器实现如何定义
2. 例外情况：某些与特定系统相关的低层编程，要求使用reinterpert_cast在端口串行输入和输出数据，或者转换某些地址中的整数，最大限度地减少使用不安全的强制转换，只是将其抽象出来的一些隐藏较好的函数中使用，这样代码就能为移植做好准备，且无需太多改动。
3. 如果需要在不相关的指针类型之间进行强制转换，应该通过void*进行转换，而不要通过reinterpret_cast。
``` c++
T1* p1 = ...;
T2 * p2 = reinterpret_cast<T2*>(p1);

而应该改成：

T* p1 = ...;
void* pv = p1;
T2* p2 = static_cast<T2*>(p2);
```
## 第93条：避免对指针使用static_cast
不要对动态对象的指针使用static_cast
## 第94条：避免强制转换const
1. 强制转换const有时会导致未定义的行为
2. 如果对象的最初定义为const，强制转换掉它的常量性，将使所有保护失效，程序完全处于未定义行为状态。
3. C++有一个隐式的cosnt_cast，可以将字符串常量转换为char*:  char* weird = "Trick or treat?";

编译器将不加提示地执行const_cast，将其转换为char*，而这样做是为了保持与C语言的API的兼容性
4. 例外情况：在调用常量不正确的API时，强制转换掉const可能是必需的。
## 第95条：不要使用C风格的强制转换
1. C风格强制转换的问题在于：同一种语法会根据所包含的文件等等奇怪因素产生微妙差异的不同作用。
2. C++风格强制转换可以避免多义现象，能够清晰地说明意图，容易查找
3. C++风格的强制转换可以很容易地使用grep这样的自动化工具进行查找。
## 第96条：不要对非POD进行memcpy操作或者memcmp操作
1. 不要使用memcpy或者memcmp来复制或比较任何对象，除非有什么对象的布局就是原始内存
2. 在多重继承情况中，对象在不同偏移位置会有多个vptr共存，使用虚拟继承时，大多数编译器会添加更多内部指针。一般使用中，由编译器负责管理这些隐藏数据
## 第97条：不要使用联合重新解释表示方式
1. 通过在union中写入一个成员而读取另一个的滥用方式可以获得“无需强制转换的强制转换”，这种方式更难预测
2. 除最后写入的字段之外不要读取union中的其他字段，读取这样的字段会导致未定义行为
3. 例外情况：如果两个PODstruct是一个union的成员，而且均以相同的字段类型开始，那么对这种匹配的字段来说，写入其中一个而读取另一个是合法的。
## 第98条：不要使用可变长参数（...）
可变长参数有很多严重缺点：
1. 缺乏类型安全性
2. 调用者与被调用者存在紧密耦合，而且需要手动协调。
3. 类类型对象的行为未定义。C++中通过可变长参数传递基本类型或者POD类型之外的任何东西，其行为都是未定义的，而且大多数编译器还不会发出任何警告
4. 参数的数量未知
## 第99条：不要使用失效对象，不要使用不安全函数
失效对象有三种：
1. 已销毁对象。超出作用域的自动对象和已经删除堆中分配的对象
2. 语义失效对象。指向已删除对象的虚悬（dangling）指针和失效迭代器，除了给失效对象赋予另一个有效值之外，任何其他操作通常都是未定义的，不安全的。
3. 从来都失效的对象。通过伪造指针获得的对象或者越界访问数组
## 第100条：不要多态地处理数组。
要存储多态对象的数组，需要用到一个基类指针的数组，这样数组中的每个指针都指向一个多态对象，或者如果要村委有多态对象的容器公用接口，那么需要封装整个数组，并提供一个多态的接口进行迭代。