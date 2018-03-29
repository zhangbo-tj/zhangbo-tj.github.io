---
layout: post
title: 设计模式-工厂模式
category: 设计模式
tags: 设计模式 工厂模式
Keywords: 工厂模式
description:
---
## 1.简单工厂模式
1. 简单工厂模式是工厂模式中最简单的一种
2. 客户端只需要告诉工厂类所需要的类型，工厂类就会返回需要的产品类，但是客户端看到的只是产品的抽象对象，并不需要关心到底是返回了哪个子类。
3. 客户端唯一需要知道的具体子类就是工厂子类
4. 缺点：要增加新的核类型时，就需要修改工厂类，这样就违反类开放封闭原则（软件实体（类、模块、函数）可以扩展，但是不可以修改）
5. 示例代码：

``` c++
enum CTYPE{
    COREA,COREB
};

class SingleCore{
public:
    virtual void Show() = 0;
};

class SingleCoreA:public SingleCore{
public:
    void Show(){
        cout << "SingleCore A"<<endl;
    }
};

class SingleCoreB:public SingleCore{
public:
    void Show(){
        cout << "SingleCore B"<<endl;
    }
};

class Factory{
public:
    SingleCore* CreateSingleCore(enum CTYPE ctype){
        switch (ctype){
            case COREA:
                return new SingleCoreA();
            case COREB:
                return new SingleCoreB();
            default:
                return nullptr;
        }
    }
};
```

## 2.工厂方法模式
1. 定义一个用于创建对象的接口，让子类决定实例化哪一个类，使一个类的实例化延迟到其子类。
2. 缺点：每增加一种产品，就需要增加一个对象的工厂
3. 适用场景：适用于大部分地方都只使用其中一种产品，工厂类也不用频繁创建产品类的情况，这样修改的时候只需要修改有限的几个地方即可。
4. 
``` c++
class SingleCore {
public:
    virtual void Show() = 0;
};

class SingleCoreA : public SingleCore {
public:
    void Show() {
        cout << "SingleCore A" << endl;
    }
};

class SingleCoreB : public SingleCore {
public:
    void Show() {
        cout << "SingleCore B" << endl;
    }
};

class Factory {
public:
    virtual SingleCore *CreateSingleCore() = 0;
};


class FactoryA : public Factory {
public:
    SingleCoreA *CreateSingleCore() {
        return new SingleCoreA();
    }
};

class FactoryB : public Factory {
    SingleCoreB *CreateSingleCore() {
        return new SingleCoreB();
    }
};
```

## 3.抽象工厂模式
1. 抽象工厂模式提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
2. 缺点：代码过于臃肿;每次如果添加一组茶品，那么所有的工厂类就必须添加一个方法，这样违背了开放-封闭原则，所以一般适用于产品组合产品族变化不大的情况。
``` c++
class SingleCore {
public:
    virtual void Show() = 0;
};

class SingleCoreA : public SingleCore {
public:
    void Show() {
        cout << "SingleCore A" << endl;
    }
};

class SingleCoreB : public SingleCore {
public:
    void Show() {
        cout << "SingleCore B" << endl;
    }
};

class MultiCore {
public:
    virtual void Show() = 0;
};

class MultiCoreA : public MultiCore {
public:
    void Show() {
        cout << "Multi Core A" << endl;
    }
};

class MultiCoreB : public MultiCore {
public:
    void Show() {
        cout << "Multi Core B" << endl;
    }
};
class CoreFactory {
public:
    virtual SingleCore *CreateSingleCore() = 0;

    virtual MultiCore *CreateMultiCore() = 0;
};
class FactoryA : public CoreFactory {
public:
    SingleCore *CreateSingleCore() {
        return new SingleCoreA();
    }

    MultiCore *CreateMultiCore() {
        return new MultiCoreA();
    }
};
class FactoryB : public CoreFactory {
public:
    SingleCore *CreateSingleCore() {
        return new SingleCoreB();
    }

    MultiCore *CreatingMultiCore() {
        return new MultiCoreB();
    }
};
```