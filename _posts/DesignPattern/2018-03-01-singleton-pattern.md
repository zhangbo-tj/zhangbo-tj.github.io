---
layout: post
title: 设计模式-单例模式
category: 设计模式
tags: 设计模式 单例模式
Keywords: 单例模式
description:
---

## 1. 简介
1. 作用：就是保证在整个应用程序的生命周期中，任何一个时刻，单例类的实例都只存在一个
2. 定义：保证一个类只有一个实例，同时这个类还必须提供一个访问该类的全局访问点
3. 特点：单例类的构造函数时私有的，外部程序如果想要访问这个单例类的话，必须通过GetInstance()来请求得到这个单例类的实例
## 2. Singleton类
``` c++
#include <iostream>

using namespace std;

class Singleton {
private:
    //定义私有的全局变量来保存该类的唯一实例
    static Singleton *instance;

private:
    //私有构造函数，这样在外部无法使用new来创建类的实例
    Singleton() {
    }

public:
    //定义全局访问点，设置为静态方法，这样在外部无需实例化就可以调用该方法
    static Singleton *GetInstance() {
        //保证只实例化一次
        if (instance == NULL) {
            instance = new Singleton();
        }
        return instance;
    }
};

Singleton *Singleton::instance = NULL;

int main() {

    Singleton *singleton1 = Singleton::GetInstance();
    Singleton *singleton2 = singleton1->GetInstance();

    if (singleton1 == singleton2) {
        cout << "They are the same instance" << std::endl;
    } else {
        cout << "They are different instance" << std::endl;
    }
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```
## 3.多线程环境的Singleton类
双重锁定（double lock）:避免两个线程重复创建实例的过程
``` c++
 //定义全局访问点，设置为静态方法，这样在外部无需实例化就可以调用该方法
    static Singleton *GetInstance() {
        //双重锁定，确保不会重复创建
        if (instance == NULL) {
            Lock();
            if(instance == NULL){
                instance = new Singleton();
            }
            Unlock();
        }
        return instance;
    }
```
## 4.懒汉单例模式
懒汉模式，就是单例类的唯一实例是在第一次使用GetInstance()时实例化的。如果不调用GetInstance()的话，它自己是不会实例化的。
</br>
懒汉模式是以时间换空间的方式
``` c++
 //定义全局访问点，设置为静态方法，这样在外部无需实例化就可以调用该方法
    static Singleton *GetInstance() {
        //双重锁定，确保不会重复创建
        if (instance == NULL) {
            Lock();
            if(instance == NULL){
                instance = new Singleton();
            }
            Unlock();
        }
        return instance;
    }
```

## 5.饿汉单例模式
饿汉模式：主动实例化单例类的唯一实例，是线程安装的。
``` c++
class Singleton{
private:
    static Singleton* instance;

private:
    Singleton(){

    }

public:
    static Singleton* GetInstance(){
        return instance;
    }
};


Singleton *Singleton::instance = new Singleton();
```
或者
``` c++
class Singleton{
private:
    static Singleton* instance;

private:
    Singleton(){

    }

public:
    static Singleton* GetInstance(){
        static Singleton* instance;
        return instance;
    }
};
```

## 6. 应用场景
1. 单例模式常常与工厂模式结合使用，因为工厂只需要创建产品实例就可以了，在多线程的环境下也不会造成任何的冲突，因此只需要一个工厂实例就可以了。