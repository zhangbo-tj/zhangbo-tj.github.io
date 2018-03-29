---
layout: post
title: C++并发 - Thread 和 Promise
category: CPP11新特性
tags: C++  并发  Thread Promise
Keywords: 并发 async Thread Promise
description:
---
## 1. thread 和async( )对比之下不提供哪些性质：
1. Class thread 没有所谓发射策略（launch policy）,而是永远尝试着将目标函数启动于一个新线程中。如果无法做到就抛出std::system_error并带有差错码resource_unavailable_try_again
2. 没有接口可处理线程结果，唯一一个可获得的是独一无二的线程ID
3. 如果发生异常，但是没有被捕捉于线程内，程序会立刻中止并调用std::terminate( )。如果想把异常传播到线程外的某个context，必须使用exception_ptr
4. 必须将线程声明为join( 等待线程结束 ) ,  detach( 运行于后台而不受任何控制 ),
5. 如果让线程运行于后台而main( )结束了，则所有线程会被鲁莽而硬性地终止。

## 2. 当心Detached Thread(卸离后的线程)：
1. 因为丧失了对detached thread的控制，没有轻松的办法可以得知它是否运行，以及运行多久。因此绝对不要让一个detached thread访问任何寿命已经结束的object, 基于这个理由，“以by reference方式传递变量和object”给线程总是有风险，应尽量以by value方式传递。
2. detached thread应该宁可只访问local copy
3. 如果detached thread用上了一个global 或static object, 应该做以下事情之一：
* 确保这些global/static object在被销毁前，所有对它们进行访问的detached thread都结束，或者是不再访问它们。

一种做法是使用condition variable，它让detached thread发信号来说它们已经结束，离开main或调用exit( )之前必须先设置这些condition variable, 然后发信号说可以进行析构了
* 以调用quick_exit( )的方式结束程序，这个函数之所以存在完全是为了以“不调用global和static object析构函数”的方式结束程序
4. 由于std::cin, std::cout , std::cerr, 以及其他global stream object按照标准是“在程序运行期间不会被销毁”，所以detached thread访问这些object应该不会导致不可预期的行为，然而其他问题像是“interleaved character”却有可能发生。
5. 终止detached thread的唯一安全方法就是搭配notify_all_at_thread_exit()， 这回强制main thread等待detached thread 真正结束。
## 3.  this_thread namespace:
1. yield( ) : 挂起当前线程的运行，给其他线程运行的机会，应该在一个线程等待其他线程资源时不阻塞地调用
2. get_id( )： 返回当前线程的thread id 
3. sleep_for( )  ： 停止线程，直到时间点段结束
4. sleep_until( ):  停止线程，直到某一时间点

## 4. promise object
promise object是future object的配对兄弟，二者都能暂时持有一个shared state,但是future object 允许你取回数据（通过调用get( )），而promise object却是让你提供数据。

promise时用来传递运行结果和异常的一般性机制。
## 4. 使用方法：
1. 声明一个promise object, 针对持有值或返回值进行特化，如果二者都是none ,就特化为void:
std::promise<string> p;
2.  将promise object的引用传递给线程中运行的任务
``` c++
thread t(DoSomething, ref(p));
```
3. 在线程中调用set_value( )或者是set_exception( current_exception())。

如果想令shared state在线程确实结束时变为ready, 以确保线程的local object以及其他材料在结果被处理之前所有清除，则最好是调用set_value_at_thread_exit( )或set_exception_at_thread_exit( )
``` c++
p.set_value(move(s));
p.set_exception(current_exception());
```
4. 一旦promise object的shared state存在某个值或某个异常，其状态就转变为ready, 就可以在其他地方去除其内容，但是这个“取出”动作需要借助一个“共享相同shared state”的future object。
``` c++
future<string> f(p.get_future());
```
5. 然后调用future.get( )就可以获得被存储的结果，或者是被存储的异常。 get()会导致block, 直到shared state 变为ready，但是此时该线程并不一定结束，该线程仍然可以继续执行其他语句，甚至存储其他结果放到promise中。
## 5. Promise & 多线程
promise和future并不局限于解决多线程问题，在单线程程序中也可以使用promise只有一个结果值或一个异常。

不能既存储值又存储异常，企图这么做会导致std::future_error

