---
layout: post
title: Python基础：面向对象
category: Python基础
tags: Python 面向对象
Keywords: Python 面向对象
description:
---

1. 类的定义：
```python
class Student(Person): #Person为要继承的类 
```
所有的类都继承自object类
2. 类似__xxx__的变量是特殊变量，可以直接访问
3. 判断对象类型用type()函数，判断class的类型用isinstance(var,class_name)函数
4. 使用dir()函数：
    (1) 如果要获取一个对象的所有属性和方法，可以使用dir()函数，函数返回一个包含字符串的List
    (2) 调用
