---
layout: post
title: C++编程规范：STL算法
category: C++编程规范
tags: 编程规范
Keywords: C++
description:
---
## 第83条：使用带检查的STL实现
1. 即使发行软件的时候不会打开检查，即使只有一个目标平台有带检查的STL，至少也要确保会例行地对带检查的STL所构建的程序版本运行完整测试。
2. STL错误：
    * 使用已失效的或未初始化的迭代器
    * 传递越界索引
    * 使用并非是范围的迭代器范围，传递的两个迭代器中，第一个并不在第二个之前，或者两个所指的并非同一个容器
    * 传递无效的迭代器位置，调用一个以迭代器位置为参数的容器成员时，传递的迭代器却指向了另外一个容器
## 第84条：用算法调用代替手工编写的循环
## 第85条：使用正确的STL查找算法
1. 对无序范围而言，find/find_if和count/count_if都能以线性事件告知，元素是否在某个范围中，以及元素在范围中的那个位置。
2. 对有序范围来说，应该使用binary_search, lower_bound, upper_bound, equal_range，因为它们都是对数时间的。

binary_search只能返回一个bool值表示是否找到了匹配lower_bound将返回一个迭代器，指向第一个匹配，或者是指向匹配应该处于的位置，

upper_bound将返回一个迭代器，指向最后一个匹配的下一个元素，也是添加下一个等价元素的位置。
## 第86条：使用正确的STL排序算法
1. 按照开销从小到大的顺序，排序算法为：partition, stable_partition,  nth_element, partial_sort, sort , stable_sort
2. partition可以将范围恰好分为两组：前面是满足谓词的所有元素，后面是不满足谓词的所有元素
3. nth_element可以将一个元素放在范围完全排序后它应该处在的第n个位置，同时所有其他元素也会正确地处于第n个元素的前后，而且其他元素是未排序的
4. partial_sort可以完成nth_element的工作之外，还能使第n个元素之前的元素都处在正确的排序位置上。
## 第87条：使谓词成为纯函数
1. 如果函数的结果只取决于其参数，则该函数就是一个纯函数
2. 不要让谓词保存或访问对其operator()结果有影响的状态，包括成员状态和全局状态，应该使operator()成为谓词的const成员函数 
3. 对于要求值的相同参数集，谓词返回的结果必须始终相同
4. 只有在以下情况下，才可以使用有状态谓词：
    * 谓词不会被复制，标准算法没有做出任何这样的保证，事实上它们假定谓词是可以安全复制的
    * 谓词是按照文献记载的确定顺序使用的，但是标准算法对谓词应以什么顺序应用于范围中的元素，没有任何保证
## 第88条：算法和比较器的参数应多用函数对象少用函数
1. 对象的适配性比函数好，应该向算法传递函数对象，而非函数
2. 指定关联函数比较器时，需要使用的是函数对象，而不是函数，这是因为直接用函数实例化模板类型参数是非法的。
``` c++
struct CompareThings:public binary_function<Thing,Thing,bool>{
  bool operator()(const Thing&, const Thing&) const;
}

set<Thing, CompareThings> s;
或者是：
inline bool isHeavy(const Thing&){ }
find_if(v.beign(),v.end(), not1(ptr_fun(isHeavy)));
```
## 第89条：正确使用函数对象
1. 将函数对象设计为复制成本很低的值类型，尽可能地让他们从unary_function或者binary_function继承，从而能够适配
2. 封装类应该具备以下特征：
    * 可适配。从unary_function或者binary_function继承
    * 具有Piml。存储一个指向大型对象实现的指针
    * 具有函数调用操作符。将这些调用传递给实现对象。
3. 要避免提供多个operator()函数，这样会加大可适配的困难程度
4. 并非所有函数对象都是谓词，谓词只是函数对象的一个子集而已。
