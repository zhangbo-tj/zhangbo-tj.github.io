---
layout: post
title: C++编程规范：组织和策略问题
category: CPP编程规范
tags: 编程规范
Keywords: C++
description:
---
## 第0条：不要拘泥于小节（了解哪些东西不应该标准化）
1. 永远不要使用“晦涩的名称”，即以下划线开始或包含双下划线的名称
2. 总是使用形如ONLY_UPPERCASE_NAMES的全大写字母表示宏，不要考虑使用常见的词或者缩略语作为宏的名字
3. 类、函数和枚举的名称应该单词首字母大写；变量名首字母小写，第二个字母大写；私有成员变量形如likeThis_
## 第1条：在高警告级别干净利落地进行编译
1. 排除警告的正确做法是：1）把它弄清楚；2）改写代码以排除警告，并使代码阅读者和编译器都能更加清楚
2. 无法修改的库文件可能包含引起警告（可能是良性的）的构造，如果这样可以用自己的包含原文件的版本将此文件包含起来，并有选择地为该作用域关闭警告，然后在整个项目的其他地方包含此包装文件（各种编译器的警告控制语法并不一样）。
``` cpp
#pragma warning(push)   //仅禁用此头文件
    #pragma warning(disable:4512)   //禁用特定警告
    #pragma warning(disable:4180)
    #include <boost/lambda/lambda.hpp>
#pragma warning(pop)    //恢复最初的警告级别
```
3. 未使用的函数参数(Unusued function parameter)如果确实不需要，则直接删除函数参数名，只保留类型名即可。
```cpp
pointer allocate(size_type numObjects, const void* /*localityHInt*/ = 0){
}
```
4. 定义了未使用过的变量（Variable defined but never used），如果确实不需要，经常可以通过插入一个变量本身的求值表达式，使编译器不再报警（这种求值不会影响运行时的速度）。
```cpp
void Func(){
    Lock lock;
    lock;   //消除警告
}
```
5. 变量使用之前可能未经初始化（Variable may be used without being initialized），初始化变量即可。
6. 遗漏了return语句（missing return），有的编译器要求每个分支都有return语句，即使控制流可能永远也不会到达函数的结尾。有的switch语句没有default情况，应该加上assert(false)的default情况。
```cpp
int Func(){Color c}{
    switch(c){
        case 1:
            break;
        default:
            assert(!"should never get here"); //！"string"的求值结果为false
            return -1;
    }
}
```
7. 有符号/无符号数不匹配（signed/unsigned mismatch）：通常没有必要对符号不同的整数进行比较和赋值，应该改变所操作的变量的类型，从而使类型匹配。最坏的情况下，要插入一个显示的类型转换。
## 第2条：使用自动构建系统
1. 一次按键解决问题：使用完全自动化的构建系统，无需用户干预即可构建整个项目。单操作的构建过程非常重要，它应该能将源文件可靠和可重复地转换为可以交付的软件包。
2. 构建有两个模式：增量构建和完全构建，增量构建只重新构建上次构建（增量的或完全的）以来发生更改的部分
3. 两次连续增量构建中的第二次构建不应该编写任何输出文件，否则可能会出现依赖循环
4. 成功的构建应该不产生任何警告
## 第3条：使用版本控制系统
使用版本控制系统（Version Control System，VCS），永远不要让文件长时间地登出。在新的单元测试通过之后，应该频繁登入，而且确保登入的代码不会影响构建成功
## 第4条：做代码审查
代码审查应该成为软件开发周期中的常规环节
