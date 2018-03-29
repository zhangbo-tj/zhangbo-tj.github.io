---
layout: post
title: C++并发 - async() 和 Future
category: CPP11新特性
tags: C++ 并发 async future
Keywords: 并发 async future
description:
---
## 1. 为程序添加并行处理
1. #include <future>
2. 传递某些可并行执行的函数，交给std::async( )作为一个可地哦啊用对象
3. 将执行结果赋值给一个fucture<ReturnType> object
4. 当需要那个被启动函数的执行结果，或当你想确保该函数结束，就对future< >object调用get( )
``` c++
    future<int> result1(async(Func1));
    int result2 = Func2();
    int result = result1.get() + result2;
```
注意： 这只适用于不发生data race(数据竞争)的情况下，data race指两个线程并发使用同一个数据而导致不可预期的行为:

1. future<> object如果没有调用get( )就不能保证其函数一定被调用，如果async无法立即启动它所受到的函数，就会推迟调用，使得当程序“调用get( )来表示一定要执行目标函数”时才会被调用。
2. 如果传递给async( )的函数不返回任何东西，async( )就会产出一个future<void>对象，这是fucture< >的一个偏特化版，这种情况下调用get( ) 将会返回void。
3. 传递给async( )的参数可以是任何类型的可调用对象（callable object）: 可以是函数、成员函数、函数对象或lambda, 
```std:: async( [ ]{ ... } );```
4. async( )提供一种编程环境，可以并行启动某些“稍后（当get( )被调用时）才会用到其结果”的动作。

## 2. Launch策略：
1. 可以强迫async( )绝不推迟目标函数的执行，只要明确传入一个launch策略用以指挥async( )，告诉它当它被调用时应该明确地以异步方式启动目标函数：
``` c++
std::future<long> result = std::async(std::launch::async, func1);
```
2. 如果异步调用在此处无法实现，程序会抛出一个std::system_error异常。
3. 使用async发射策略后，就不必非得调用get( )了，因为当future object离开作用域时，程序会等待后台任务结束。

尽管如此，程序结束前调用get( )会让行为更加清晰。
4. 同样，可以强制延缓执行（deferred execution）,从而允许延缓执行函数，直到调用get( )
``` c++
std::future<long> result = std::async(std::launch::deferred, func1);
```

## 3. 处理异常：
future object对象调用get( )也能处理异常，事实上当get( )被调用时，且后台操作已经终止，该异常不会在此线程内被处理，而是会被再次传播出去。因此要处理后台操作发生的异常，只需要对get( )作出以同步方式调用该操作所做的相同操作即可。
``` c++
auto f1 = async(task1);
try{
    f1.get();
}catch(const exception& e){
    cerr << "Exception: "<<e.what() <<endl;
}
```
## 4. 等待和轮询（Waiting and Polling）
1. 一个future< >只能调用get( )一次，之后future就处于无效状态，而这种状态只能借由“对future调用valid( )”来检测，之后对它的任何调用（析构除外）会导致不可预期的行为。
2. future提供一个接口，允许等待后台操作完成而不需要处理其结果，这个接口可被调用一次以上；也可以结合一个duration( 时间段)或timepoint( 时间点 )以限制等待时间。
3. 强制启动线程并等待后台操作完成：
``` c++
std::future<...> f(async(func));
...
f.wait(); //等待函数调用完成
```
4. 函数中的操作等待一段时间：
``` c++
std::future<...> f(async(func));
f.wait_for(std::chrono::seconds(10)); //函数func内的操作等待10s
```
5. 函数中的操作等待直到达到某特定时间点：
```
std::future<...> f(async(func));
f.wait_until(std::system_clock::now() + std::chrono::minutes(1));
```
6. future< >的wait_for( )和wait_until( )都返回以下三个东西之一：
* std::future_status::deferred: 如果async( )延缓了操作而程序中又完全没有调用wait( )或get( )，这种情况下上述两个函数都会立刻返回
* std::future_status::timeout : 如果某个操作被异步启动但尚未结束，而waiting有已逾期，则返回
* std::future_status::ready : 如果操作已经完成
7. 如果传入一个zero时间段，或者是一个过去事件点，则就可以仅轮询（poll）是否有个后台任务已经启动，或者是否它正在运行中：
``` c++
std::future<...> f(async(func));
while(f.wait_for(chrono::seconds(0) != future_status::ready)){

}
```
## 5. 传递参数
1. 使用lambda:
传值：
``` c++
char c = 'a';
auto f = async([=]{ DoSomething(c);});
```
传引用：需要保持参数的生命，直到后台操作完成为止 
```
char c = 'a';
auto f = async([&]{ DoSomething(c);});
```
2. 使用async 传递参数：传值方式
char c = 'a';
auto f = async(DoSomething, c);
## 6. Shared Future
1. 所有shared future object共享所谓shared state, 用来存放目标函数的运行结果。 所以shared_future可以多次调用get( )， 导致相同结果，或者是导致抛出同一个异常。
2. 创建shared future的方式：
* 通过async( )函数创建（不能用auto）： ```shared_future<int>  f =  async(func);```
* 通过.share( )函数创建( 可以用auto ) ：  ```auto  f = async( func ).share( );```
3. 可以通过传递引用的方式传递shared_future,但是这种情况就不再是“使用多个shared future object而由它们共享同一个shared state”,而是“使用shared future object执行多次get( )”。然而这种做法风险较高，因为此时必须要确保shared_future 对象的寿命。
