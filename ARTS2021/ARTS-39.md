---
layout: post
title: ARST(三十九)
date: 2021-01-11 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:123. Best Time to Buy and Sell Stock III】**

##### 　　Description:
　　You are given an array prices where prices[i] is the price of a given stock on the ith day.  

　　Find the maximum profit you can achieve. You may complete at most two transactions.  

　　Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).  

##### 　　Example 1:
```sh
Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

##### 　　Example 2:
```sh
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
```

##### 　　Example 3:
```sh
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

##### 　　Example 4:
```sh
Input: prices = [1]
Output: 0
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/123.BestTimeToBuyAndSellStockIII/BestTimeToBuyAndSellStockIII.cpp  


###  <font color=yellow>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/c-20-concepts-the-details  
　　C++20: Concepts, the Details  

#### **【Words】**
English | Chinese
-|-
Arithmetic | n. 算术，算法；数字

#### **【Comments and Conclusions】**

##### The Details
　　Just do keep it in mind: What are the advantages of concepts?  

　　- Requirements for templates are part of the interface.  
　　- The overloading of functions or specialisation of class templates can be based on concepts.  
　　- We get improved error message because the compiler compares the requirements of the template parameter with the actual template arguments.  
　　- You can use predefined concepts or define your own.  
　　- The usage of auto and concepts is unified. Instead of auto, you can use a concept.  
　　- If a function declaration uses a concept, it automatically becomes a function template. Writing function templates is, therefore, as easy as writing a function.  

##### Three Ways
　　There are three ways to use the concept Sortable. For simplicity reasons, I only show the declaration of the function template.  

　　1. Requires Clause  
```c++
template <typename Cont>
requires Sortable<Cont> void sort(Cont& container);
```

　　2. Trailing Requires Clause  
```c++
template <typename Cont>
void sort(Cont& container) requires Sortable<Cont>;
```

　　3. Constrained Template Parameters  
```c++
template<Sortable Cont>
void sort(Cont& container)
```

##### Classes
　　You can define a class template which only accepts objects.  
```c++
template <Object T>
class MyVector {};

MyVector<int> v1;   // OK
MyVector<int&> v2;  // ERROR: int& does not satisfy the constraint Object
```

　　The compiler complains that and reference is not an object. Maybe you wonder, what an object is.? A possible implementation der type-traits function std::is_object gives the answer:  
```c++
template <class T>
struct is_object : std::integral_constant<bool, std::is_scalar<T>::value || std::is_array<T>::value ||
                                                    std::is_union<T>::value || std::is_class<T>::value> {};
```

##### Member Functions
```c++
template <Object T>
class MyVector {
    void push_back(const T& e) requires Copyable<T>{};
};
```

　　In this case, the member function requires that the template parameter T must be copyable.  

##### Variadic Templates
```c++
#include <iostream>
#include <type_traits>

template <typename T>
concept Arithmetic = std::is_arithmetic<T>::value;

template <Arithmetic... Args>
bool all(Args... args) {
    return (... && args);
}

template <Arithmetic... Args>
bool any(Args... args) {
    return (... || args);
}

template <Arithmetic... Args>
bool none(Args... args) {
    return !(... || args);
}

int main() {
    std::cout << std::boolalpha << std::endl;
    std::cout << "all(5, true, 5.5, false): " << all(5, true, 5.5, false) << std::endl;
    std::cout << "any(5, true, 5.5, false): " << any(5, true, 5.5, false) << std::endl;
    std::cout << "none(5, true, 5.5, false): " << none(5, true, 5.5, false) << std::endl;
    return 0;
}
```
　　You can use concepts in variadic templates.  The definition of the function templates are based on fold expressions. all, any, and none requires from it type parameter T that is has to support the concept Arithmetic.  

##### More Requirements
```c++
template <SequenceContainer S, EqualityComparable<value_type<S>> T>
Iterator_type<S> find(S&& seq, const T& val) {
    ...
}
```
　　The function template find requires that the container S is a SequenceContainer and that its elements are EqualityComparable.  

##### Overloading
```c++
template <InputIterator I>
void advance(I& iter, int n) {
    ...
}

template <BidirectionalIterator I>
void advance(I& iter, int n) {
    ...
}

template <RandomAccessIterator I>
void advance(I& iter, int n) 
{
    ...
}

// usage
std::vector<int> vec{1, 2, 3, 4, 5, 6, 7, 8, 9};
auto vecIt = vec.begin();
std::advance(vecIt, 5);  //  RandomAccessIterator

std::list<int> lst{1, 2, 3, 4, 5, 6, 7, 8, 9};
auto lstIt = lst.begin();
std::advance(lstIt, 5);  //  BidirectionalIterator

std::forward_list<int> forw{1, 2, 3, 4, 5, 6, 7, 8, 9};
auto forwIt = forw.begin();
std::advance(forwIt, 5);  //  InputIterator
```
　　Based on the iterator category, the containers std::vector, std::list, and std::forward_list support, the best fitting std::advance implementation is used.  

##### Specialisations
```c++
template<typename T>
class MyVector{};

template<Object T>
class MyVector{};

MyVector<int> v1;     // Object T
MyVector<int&> v2;    // typename T
```
　　MyVector<int&> goes to the unconstrained template parameter.  

　　MyVector<int> goes to the constrained template parameter.  

###  <font color=yellow>tips: RAID</font>

##### RAID 0
　　RAID 0 是数据在从内存缓冲区写入磁盘时，根据磁盘数量将数据分成 N 份，这些数据同时并发写入 N 块磁盘，使得数据整体写入速度是一块磁盘的 N 倍；读取的时候也一样，因此 RAID 0具有极快的数据读写速度。但是 RAID 0 不做数据备份，N 块磁盘中只要有一块损坏，数据完整性就被破坏，其他磁盘的数据也都无法使用了。  

##### RAID 1
　　RAID 1 是数据在写入磁盘时，将一份数据同时写入两块磁盘，这样任何一块磁盘损坏都不会导致数据丢失，插入一块新磁盘就可以通过复制数据的方式自动修复，具有极高的可靠性。  

##### RAID 10
　　结合 RAID 0 和 RAID 1 两种方案构成了 RAID 10，它是将所有磁盘 N 平均分成两份，数据同时在两份磁盘写入，相当于 RAID 1；但是平分成两份，在每一份磁盘（也就是 N/2 块磁盘）里面，利用 RAID 0 技术并发读写，这样既提高可靠性又改善性能。不过 RAID 10 的磁盘利用率较低，有一半的磁盘用来写备份数据。  

##### RAID 3
　　RAID 3 可以在数据写入磁盘的时候，将数据分成 N-1 份，并发写入 N-1 块磁盘，并在第 N 块磁盘记录校验数据，这样任何一块磁盘损坏（包括校验数据磁盘），都可以利用其他 N-1 块磁盘的数据修复。但是在数据修改较多的场景中，任何磁盘数据的修改，都会导致第 N 块磁盘重写校验数据。频繁写入的后果是第 N 块磁盘比其他磁盘更容易损坏，需要频繁更换，所以 RAID 3很少在实践中使用。  

##### RAID 5
　　RAID 5 是使用更多的方案。RAID 5 和 RAID 3 很相似，但是校验数据不是写入第 N 块磁盘，而是螺旋式地写入所有磁盘中。这样校验数据的修改也被平均到所有磁盘上，避免 RAID 3频繁写坏一块磁盘的情况。  

##### RAID 6
　　RAID 6 和 RAID 5 类似，但是数据只写入 N-2 块磁盘，并螺旋式地在两块磁盘中写入校验信息（使用不同算法生成）。  

　　从下面表格中你可以看到在相同磁盘数目（N）的情况下，各种 RAID 技术的比较。  
![hello](/ARTS2021/ARTS-39/5.jpg)    

###  <font color=yellow>Share: 单服务器高性能模式：PPC与TPC</font>
　　单服务器高性能的关键之一就是服务器采取的并发模型，并发模型有如下两个关键设计点：  

　　- 服务器如何管理连接。  
　　- 服务器如何处理请求。  

　　以上两个设计点最终都和操作系统的 I/O 模型及进程模型相关。  

　　- I/O 模型：阻塞、非阻塞、同步、异步。  
　　- 进程模型：单进程、多进程、多线程。  

#### **【PPC】**
　　PPC 是 Process Per Connection 的缩写，其含义是指每次有新的连接就新建一个进程去专门处理这个连接的请求，这是传统的 UNIX 网络服务器所采用的模型。基本的流程图是：  
![hello](/ARTS2021/ARTS-39/1.jpg)  

　　- 父进程接受连接（图中 accept）。  
　　- 父进程“fork”子进程（图中 fork）。  
　　- 子进程处理连接的读写请求（图中子进程 read、业务处理、write）。  
　　- 子进程关闭连接（图中子进程中的 close）。  

　　缺点：  

　　1. fork 代价高：站在操作系统的角度，创建一个进程的代价是很高的，需要分配很多内核资源，需要将内存映像从父进程复制到子进程。  
　　2. 父子进程通信复杂：父进程“fork”子进程时，文件描述符可以通过内存映像复制从父进程传到子进程，但“fork”完成后，父子进程通信就比较麻烦了，需要采用 IPC（Interprocess Communication）之类的进程通信方案。  
　　3. 支持的并发连接数量有限：如果每个连接存活时间比较长，而且新的连接又源源不断的进来，则进程数量会越来越多，操作系统进程调度和切换的频率也越来越高，系统的压力也会越来越大。  

#### **【prefork】**
　　PPC 模式中，当连接进来时才 fork 新进程来处理连接请求，由于 fork 进程代价高，用户访问时可能感觉比较慢，prefork 模式的出现就是为了解决这个问题。  

　　顾名思义，prefork 就是提前创建进程（pre-fork）。系统在启动的时候就预先创建好进程，然后才开始接受用户的请求，当有新的连接进来的时候，就可以省去 fork 进程的操作，让用户访问更快、体验更好。prefork 的基本示意图是：  
![hello](/ARTS2021/ARTS-39/2.jpg)  

　　prefork 的实现关键就是多个子进程都 accept 同一个 socket，当有新的连接进入时，操作系统保证只有一个进程能最后 accept 成功。但这里也存在一个小小的问题：“惊群”现象，就是指虽然只有一个子进程能 accept 成功，但所有阻塞在 accept 上的子进程都会被唤醒，这样就导致了不必要的进程调度和上下文切换了。幸运的是，操作系统可以解决这个问题，例如 Linux 2.6 版本后内核已经解决了 accept 惊群问题。  

　　prefork 模式和 PPC 一样，还是存在父子进程通信复杂、支持的并发连接数量有限的问题，因此目前实际应用也不多。Apache 服务器提供了 MPM prefork 模式，推荐在需要可靠性或者与旧软件兼容的站点时采用这种模式，默认情况下最大支持 256 个并发连接。  

#### **【TPC】**
　　TPC 是 Thread Per Connection 的缩写，其含义是指每次有新的连接就新建一个线程去专门处理这个连接的请求。与进程相比，线程更轻量级，创建线程的消耗比进程要少得多；同时多线程是共享进程内存空间的，线程通信相比进程通信更简单。因此，TPC 实际上是解决或者弱化了 PPC fork 代价高的问题和父子进程通信复杂的问题。  

　　TPC 的基本流程是：  
![hello](/ARTS2021/ARTS-39/3.jpg)  

　　- 父进程接受连接（图中 accept）。  
　　- 父进程创建子线程（图中 pthread）。  
　　- 子线程处理连接的读写请求（图中子线程 read、业务处理、write）。  
　　- 子线程关闭连接（图中子线程中的 close）。  

　　缺点：  

　　1. 创建线程虽然比创建进程代价低，但并不是没有代价，高并发时（例如每秒上万连接）还是有性能问题。  
　　2. 无须进程间通信，但是线程间的互斥和共享又引入了复杂度，可能一不小心就导致了死锁问题。  
　　3. 多线程会出现互相影响的情况，某个线程出现异常时，可能导致整个进程退出（例如内存越界）。  
　　4. 存在 CPU 线程调度和切换代价的问题。  

#### **【prethread】**
　　TPC 模式中，当连接进来时才创建新的线程来处理连接请求，虽然创建线程比创建进程要更加轻量级，但还是有一定的代价，而 prethread 模式就是为了解决这个问题。  

　　和 prefork 类似，prethread 模式会预先创建线程，然后才开始接受用户的请求，当有新的连接进来的时候，就可以省去创建线程的操作，让用户感觉更快、体验更好。  

　　由于多线程之间数据共享和通信比较方便，因此实际上 prethread 的实现方式相比 prefork 要灵活一些，常见的实现方式有下面几种：  

　　- 主进程 accept，然后将连接交给某个线程处理。  
　　- 子线程都尝试去 accept，最终只有一个线程 accept 成功，方案的基本示意图如下：  
![hello](/ARTS2021/ARTS-39/4.jpg)  

#### **【脑图】**
![hello](/ARTS2021/ARTS-39/5.png)  

#### **【实现】**
https://github.com/mmdaozhu/high-performance-server-model  

