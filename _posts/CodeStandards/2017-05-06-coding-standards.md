---
layout: post
title: C++编程规范：类的继承与设计2
category: CPP编程规范
tags: 编程规范
Keywords: C++
description:
---
## 第41条：将数据成员设为私有的，无行为的聚集
1. 避免将公有数据和非公有数据混合在一起
2. 私有数据都是类用来保持其不变式的最佳方式如果类要建模一个对象，并因而必须维持不变式，那么使用公用数据就不好了。拥有公用数据意味着类的部分状态的变化可能是无法控制的、无法预测的、与其他状态异步发生的。这意味抽象将与使用抽象的所有代码组成的无限集合共同承担维持一个或多个不变式的职责。

保护数据具有公用数据的所有缺点，因为拥有保护数据仍然意味着抽象将与代码的无限集合共同承担维持一个或多个不变式的职责，只不过这里的集合是由当前派生类和未来的派生类组成的。
3. 在同一个类中混合使用公用和非公用数据成员既容易含混不清，又容易前后矛盾。私有数据表明具有不变式而且希望保持这种不变性，而将其与公用数据混合则意味着无法明确地判定这个类到底是不是要成为抽象。
4. 如果允许调用代码直接操作内部数据，则会对它们所提供的抽象和必须维持的不变式直接产生消极的影响。
5. 如果没有更好的领域抽象可供选择，至少还可以将公用和保护数据成员放在get和set函数后面，变为私有和隐藏的，这些函数提供来了最小的抽象和健壮的版本管理机制。
6. 例外情况：值的聚集只是将一组数据简单地放在了一起，但是并没有实际添加什么有效的行为或者要建模什么抽象并实施不变式，它们并不是想提供抽象。它们的数据成员应该都是公用的，因为数据成员本身就是接口。
## 第42条：不要公开内部数据
1. 避免返回类所管理的内部数据的句柄
2. 类不能改变其抽象的低层实现，因为客户将依赖于此
3. 例外情况：std::basic_string通过成员函数c_str()和data提供了访问其内部句柄的方式，以兼容要接受C语言形式指针为参数的函数，但是不会存储指针或通过它们写入数据。
## 第43条：明智地使用Piml
1. 应该用PIML来存储所有的私有成员，包括成员数据和成员函数，这样可以使我们能够随意改变类的私有实现细节，而不用重新编译调用代码
2. 对于通过值保存的私有数据成员以及通过值接口的或者用于可见函数实现的私有成员函数中的参数，必须定义它们的类型，即使此编译单元中根本不需要，这会导致更长的构建时间。
``` c++
class C{
    //...
    private:
    AComplicatedType act_;
};
```
包含类C定义的头文件必须包含含有AComplicatedType定义的头文件，后者继而又要传递性地包含AComplicatedType可能需要的所有头文件。

但是在PIML中，如果向类中添加或修改实际操作部分的代码，则不需要重新编译所有的代码。
## 第44条：优先编写非成员非友元函数
尽可能将函数指定为非成员非友元函数，能够提高封装性，函数体不能依赖于类的非公用成员。
## 第45条：总是一起提供new和delete
1. 每个类专门的重载void* operator new(parms)都必须与对应的重载void operator delete(void*, parms)想对应
2. plecement new： void* T::operator new(size_t, void*p){return p;}并不需要对应的operator delete,因为没有进行真正的分配，而且所有编译器对void T::operator delete(void*, size_t, void*)都不会发出警告。
## 第46条：如果提供类专门的new,应该提供所有标准形式（普通，in-place和不抛出）
1. C++中，在某个作用域里定义了一个名字之后，就会隐藏所有外围作用域中同样的名字，而且永远不会发生跨作用域的重载。如果为类operator new之一提供专门的版本，默认时类将屏蔽其他两个。
2. 在两种不同的环境下，公开已隐藏的operator new需要采用两种不同的方式
    * 如果类的基类也定义了operator new，那么要公开operator new需要做的就是： 
``` c++
class C:public B{
public:
    using B::operator new;
} 
```
    * 如果没有基类版本或者基类没有定义operator new，就需要写一些短小的转送函数，因为无法通过using从全局名字空间空导入名字。
```
class C{
public:
    static void* operator new(std::size_t s){
        return ::operator new(s);
    }

    static void* operator new(std::size_t s, std::nothrow_t nt) throw(){
        return ::operator new(s,nt);
    }

    static void* operator new(std::size_t s, void* p){
        return ::operator new(s,p);
    }
}
```
3. 避免在客户代码中调用new(nothrow)版本，但是仍然要为客户提供，以免客户一旦要用到时感到奇怪。
