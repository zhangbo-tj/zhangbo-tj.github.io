---
layout: post
title: C++并发 - Mutex 和 Lock
category: CPP11新特性
tags: C++  并发  Mutex Lock
Keywords: 并发 async Mutex Lock
description:
---
## 1. Mutex简介
Mutex的全名为mutual exclusion(互斥体)，其object用来协助采取独占且排他的方式控制“对资源（object或多个Object的组合）的并发访问”。
1. 为了获得独占式的资源访问能力，相应的线程必须锁定（lock）mutex，这样可以防止其他线程也锁定mutex, 直到第一个线程解锁Mutex
2. mutex的锁定和解锁：
``` c++
std::mutex val_mutex;
void Func(){
    val_mutex.lock();
    ...
    val_mutex.unlock();
}
```
3. 凡是可能发生concurrent access的地方都应该使用同一个mutex, 不论读或写都是如此。
4. 使用RAII守则（Resource Acquisition Is Initialization）可以避免手动lock/unlock mutex，构造函数将获得资源，而析构函数则负责释放资源，可以使用C++标准库内的std::lock_guard:
``` c++
std::mutex val_mutex;
{
    std::lock_guard<std::mutex> lg(val_mutex);
    ...
}
```
注意要将lock通过  {   }限制在可能之最短周期内。因为它们会阻塞（block）其他代码的并行运行机会。
## 2. 递归的（Recursive）Lock
recursive_mutex允许同一线程多次锁定，并在最近一次相应的unlock（）时释放lock:
``` c++
class Access{
    private:
        std::recursive_mutex db_mutex;
    public:
        void CreateTable(){
            std::lock_guard<std::recursive_mutex> lg(db_mutex);
        ...
    }
    void InsertData(){
        std::lock_guard<std::recursive_mutex> lg(db_mutex);
        ...
    }
    void CreateTableAndInsert(){
        std::lock_guard<std::recursive_mutex> lg(db_mutex);
        ...
        CreateTable();
    }
};
```
## 3. 尝试性的Lock:
1. 程序想要一个lock，但是如果不可能成功的话也不想永远阻塞（block）
2. mutex提供成员函数try_lock( ),它试图取得一个lock，成功就返回true, 失败就返回false:
``` c++
std::mutex val_mutex;
while(val_mutex.try_lock() == false){
    DoSomeOtherStuff();
}
```
## 4. 使用lock_guard的adopt_lock:
``` c++
std::mutex val_mutex;
std::lock_guard<std::mutex> lg(val_mutex, std::adopt_lock);
```
## 5. 带时间性的Lock:
1. 为了等待特定长度的时间，可以选用timed mutex, 有两个特殊的Mutex class : timed_mutex 和 recursive_times_mutex， 并额外允许调用try_lock_for( ), 或try_lock_until( )。
``` c++
std::timed_mutex val_mutex;
if(val_mutex.try_lock_for(std::chrono::seconds(1))){
    std::lock_guard<std::timed_mutex> lg(m, std::adopt_lock);
    ...
}else{
    CouldNotGetLock();
}
```
## 6. 处理多个Lock:
1. 通常一个线程只能锁定一个mutex, 但是有的时候必须要锁定多个mutex, 例如为了传送数据，从一个受保护资源到另一个。这种情况下，可能会取得第一个lock之后却取不到第二个lock, 或许会发生deadlock。
2. C++标准提供了可以锁定多个Lock的方法：
``` c++
std::mutex m1;
std::mutex m2;
{
    std::lock(m1, m2); //锁定两个mutex
    std::lock_guard<std::mutex> lock_m1(m1, std::adopt_lock);
    std::lock_guard<std::mutex> lock_m2(m2, std::adopt_lock);
    ...
    //自动unlock all mutex
}
```
全局函数lock( )会锁住它收到的所有mutex，而且阻塞(blocking)直到所有mutex都被锁定或者直到抛出异常。如果抛出异常，则所有已经被成功锁定的mutex都会被解锁。

成功锁定之后应该使用lock guard, 并且以adopt_lock作为初始化的第二实参，确保任何情况下这些mutex在离开作用域时会被解锁。
3. 尝试取得多个lock: 全局函数try_lock( )会在取得所有Lock 的情况下返回-1， 都则会返回第一个失败的Lock的索引（从0开始计），而且这种情况下所有成功的lock也都会被解锁。
``` c++
std::mutex m1;
std::mutex m2;
int idx = std::try_lock(m1, m2);
if(idx < 0){
    std::lock_guard<std::mutex> lock_m1(m1, std::adopt_lock);
    std::lock_guard<std::mutex> lock_m2(m2, std::adopt_lock);
}else{
    std::cerr << "Could not lock mutex m"<<idx + 1<<std::endl;
}
```
## 7. unique_lock:
1. unique_lock< >提供的接口和lock_guard< >相同，但是又明确写出“何时”以及“如何”锁定或解锁其mutex，因此其Object可能会拥有一个mutex。而lock_guard对象总是拥有一个mutex
2. unique_lock的优点： 如果析构时mutex仍然被锁住，其析构函数会自动调用unlock, 如果当时没有锁住mutex, 则析构函数不会做任何事。
3. 传递try_to_lock, 表示企图锁定mutex但是不希望阻塞：
``` c++
std::unique_lock<std::mutex> lock(mutex, std::try_to_lock);
if(lock){
    ....
}
```
4. 传递一个时间段或时间点，表示尝试在一个明确的时间周期内锁定：
``` c+++
std::unique_lock<std::timed_mutex>   lock(mutex,  std::chrono::seconds(1));
```
5. 可以传递defer_lock, 表示初始化这一lock object但是尚未打算锁住Mutex:
``` c++
std::unique_lock<std::mutex>  lock(mutex, std::defer_lock);
...
lock.lock( );
```
6. 以上的defer_lock可以表示用来建立一个或多个lock,并在后面才锁定它们。
``` c++
std::mutex m1;
std::mutex m2;
std::unique_lock<std::mutex> lock_m1(m1, std::defer_lock);
std::unique_lock<std::mutex> lock_m2(m2, std::defer_lock);
std::lock(m1, m2);
```
## 8. 细说
1. C++标准库提供了以下mutex class:
* mutex: 同一时间只可能被一个线程锁定，如果它被锁住，任何其他lock操作都会阻塞，，直到这个mutex再次可用，且try_lock( )会失败
* recursive_mutex: 允许在同一时间多次被同一线程获得其lock，其典型应用是函数捕获一个lock并调用另一个函数， 而后者再次捕获相同的Lock
* timed_mutex: 额外允许传递一个时间段或时间点，用来定义多长时间内它可以尝试捕获一个Lock, 为此它提供了try_lock_for( )和try_lock_until( )
* recursive_timed_mutex: 允许同一线程多次获取其lock，可指定期限。
2. 细说class lock_guard:

提供了一个很小的接口，用以确保一个locked mutex在离开作用域时总是被释放，它的生命周期总是与一个lock相关联。

3. 细说Class unique_lock:
* 为一个不一定得锁定（或拥有）的mutex提供一个lock guard, 如果它在析构期间仍旧拥有一个mutex, 那么就会调用unlock( )。
* 可以明确地控制是否它有一个关联的mutex 以及是否这个mutex被锁住（Locked）



