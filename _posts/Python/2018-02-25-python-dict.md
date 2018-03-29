---
layout: post
title: Python基础：List
category: Python基础
tags: Python List
Keywords: Python List
description:
---

1. list取元素：
list_name[stop] => 在list内取索引范围为[0,stop]的元素
list_name[start,stop] => 在list内取索引范围为[start,stop]的元素
list_name[start:stop:interval] => 在list内索引范围为[start,stop]内，以interval为间隔取元素
list_name[::interval] => 在整个list内，以interval为间隔取元素
示例：
```python
list_name[3] # 取索引范围为[0,3]内的元素
list_name[1:3] # 取索引范围为[1,3]内的元素
list_name[1,5,2] # 取索引位置为1,3,5的元素
```
2. 复制list：
```python
new_list = old_list #只拷贝了引用，但是实际上指向同一块内存
new_list = old_list[:] #slice切片
new_list = list(old_list) #使用list()函数
new_list = copy.copy(old_list) #要找出old_list的dtype,所以会比较慢
new_list = copy.deepcopy(old_list) #如果list内存在list,array等对象，则同样对其进行拷贝
```
3. 列表生成式：
```python
list(range(1,11)) #生成(1,2,3...10)
[x * x for x in range(1,11)] #生成[1*1, 2*2, .... 10*10]
[x * x for x in range(1,11) if x % 2 == 0] #生成偶数的平方[2*2, 4*4...10*10]
[m + n for m in 'ABC' for n in 'XYZ'] #两层循环
```
