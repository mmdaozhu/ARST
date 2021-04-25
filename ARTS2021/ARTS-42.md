---
layout: post
title: ARST(四十二)
date: 2021-02-01 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=green>Algorithm</font>

#### **【LeetCode:714. Best Time to Buy and Sell Stock with Transaction Fee】**

##### 　　Description:
　　You are given an array prices where prices[i] is the price of a given stock on the ith day, and an integer fee representing a transaction fee.  

　　Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.  

　　Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).  

##### 　　Example 1:
```sh
Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

##### 　　Example 2:
```sh
Input: prices = [1,3,7,5,10,3], fee = 3
Output: 6
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/714.BestTimeToBuyAndSellStockWithTransactionFee/BestTimeToBuyAndSellStockWithTransactionFee.cpp  

###  <font color=green>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/c-20-concept-syntactic-sugar  
　　C++20: Concepts - Syntactic Sugar  

#### **【Comments and Conclusions】**

##### Abbreviated Function Templates
　　In C++14, generic lambdas introduced a new way to define function templates. Just use auto as a function parameter. On the contrary, you can not use auto as a function parameter to get a function template.  
```c++
auto addLambda = [](auto fir, auto sec) { return fir + sec; };
auto addFunction(auto fir, auto sec) { return fir + sec; }
```

　　This weird behaviour is gone with C++20 and the unification is, also, extended to concepts.  
```c++
#include <iostream>
#include <type_traits>

template <typename T>  // (1)
concept Integral = std::is_integral<T>::value;

template <typename T>  // (2)
requires Integral<T> T gcd(T a, T b) {
    if (b == 0)
        return a;
    else
        return gcd(b, a % b);
}

template <typename T>  // (3)
T gcd1(T a, T b) requires Integral<T> {
    if (b == 0) {
        return a;
    } else
        return gcd(b, a % b);
}

template <Integral T>  // (4)
T gcd2(T a, T b) {
    if (b == 0) {
        return a;
    } else
        return gcd(b, a % b);
}

Integral auto gcd3(Integral auto a, Integral auto b) {  // (5)
    if (b == 0) {
        return a;
    } else
        return gcd(b, a % b);
}

auto gcd4(auto a, auto b) {  // (6)
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}

int main() {
    std::cout << "gcd(100, 10)= " << gcd(100, 10) << std::endl;
    std::cout << "gcd1(100, 10)= " << gcd1(100, 10) << std::endl;
    std::cout << "gcd2(100, 10)= " << gcd2(100, 10) << std::endl;
    std::cout << "gcd3(100, 10)= " << gcd3(100, 10) << std::endl;
    std::cout << "gcd4(100, 10)= " << gcd3(100, 10) << std::endl;
    return 0;
}
```

　　Line 1 defines the concept Integral. gcd - gcd2 (lines 2 - 4) use the concept in various ways. gcd used the requires clause, gcd1 the so-called trailing requires clause, and gcd2 constrained template parameters.  

　　With gcd3, the syntactic sugar starts. The function declaration requires from its type parameters that they support the concept Integral. This syntactic form is the new way to use a concept and to get a function template that is equivalent to the previous versions gcd - gcd2.  

　　The syntactic form of gcd3 and gcd4 is called abbreviated function templates. Integral auto in the declaration of gcd3 is a constrained placeholder (concept), but you can also use an unconstrained placeholder (auto)  in a function declaration such as in gcd4 (line 6).  

　　Using an unconstrained placeholder (auto) in a function declaration generates a function template. The following two functions add are equivalent:  
```c++
template <typename T, typename T2>
auto add(T fir, T2 sec) {
    return fir + sec;
}

auto add(auto fir, auto sec) { return fir + sec; }
```

　　The key observation is, that both arguments can have different types. The same holds for concepts.  
```c++
template <Arithmetic T, Arithmetic T2>  // (1)
auto sub(T fir, T2 sec) {
    return fir - sec;
}

Arithmetic auto sub(Arithmetic auto fir, Arithmetic auto sec) {  // (2)
    return fir - sec;
}
```

##### Overloading
　　The abbreviated function templates syntax behaves as expected. The new syntax support overloading. As usual, the compiler chooses the best fitting overload.  
```c++
#include <iostream>
#include <type_traits>

template <typename T>
concept Integral = std::is_integral<T>::value;

void overload(auto t) { std::cout << "auto : " << t << std::endl; }

void overload(Integral auto t) { std::cout << "Integral : " << t << std::endl; }

void overload(long t) { std::cout << "long : " << t << std::endl; }

int main() {
    overload(3.14);   // (1)
    overload(2010);   // (2)
    overload(2020l);  // (3)
}
// g++ -std=c++20 test.cpp -o test
// result:
// auto : 3.14
// Integral : 2010
// long : 2020
```

###  <font color=green>tips: c++面试（三）</font>

#### **【c++面试题】**

##### 1. 什么情况下析构函数必须是虚函数？为什么C++默认的析构函数不是虚函数？
　　为了实现多态而有继承关系的基类的析构函数必须是虚函数，当使用基类指针指向子类对象时，释放基类指针可以释放子类的空间，防止内存泄漏。   

　　C++默认的析构函数不是虚函数因为虚函数需要额外的虚函数表和虚函数指针，占用额外的内存。  

##### 2. 请你说说你了解的RTTI
　　即通过运行时类型识别，程序能够使用基类的指针或引用来检查着这些指针或引用所指的对象的实际派生类型。  

　　RTTI提供了两个非常有用的操作符：typeid和dynamic_cast。  

　　typeid操作符，返回指针和引用所指的实际类型。  

　　dynamic_cast用于动态类型的转换。用于含有虚函数的类层次的向上转换和向下转换。  

#### **【操作系统面试题】**

##### 1. 请你说一说线程间的同步方式
　　1. 信号量：信号量是一中特殊的变量，可用于线程同步。它只取自然值。它允许多个线程在同一时刻去访问同一个资源，但一般需要限制同一时刻访问此资源的最大线程数目。  

　　2. 互斥量：互斥量又称为互斥锁，当进入临界区时，需要获得互斥锁并且加锁；当离开临界区时，需要对互斥锁解锁，以唤醒其他等待该互斥锁的线程。  

　　3. 条件变量：条件变量提供一种线程间通信机制，当某个共享数据达到某个值时，唤醒等待这个共享数据的一个或多个线程。  

#### **【计算机网络面试题】**

##### 1. 使用HTTP长连接有哪些优点？
　　优点：  
　　1. 减少握手次数  
　　2. 减少慢启动影响  

##### 2. 服务器的最大并发连接数是多少？
　　2^32(source ip)+2^16(source port) = 2^48(约等于)  

##### 3. TCP和UDP协议该如何选择？
　　TCP：  
　　- 可靠  
　　- 传递任意长度的消息  
　　- 流量控制  
　　- 拥塞控制  

　　UDP：  
　　- 一对多通讯  
　　- 效率高  
　　- 简单  
　　- 实时性更好  
　　- 无对头阻塞问题  

###  <font color=green>Share: 高性能负载均衡：算法</font>
　　根据算法期望达到的目的，大体上可以分为下面几类。  

　　任务平分类：负载均衡系统将收到的任务平均分配给服务器进行处理，这里的“平均”可以是绝对数量的平均，也可以是比例或者权重上的平均。  

　　负载均衡类：负载均衡系统根据服务器的负载来进行分配，这里的负载并不一定是通常意义上我们说的“CPU 负载”，而是系统当前的压力，可以用 CPU 负载来衡量，也可以用连接数、I/O 使用率、网卡吞吐量等来衡量系统的压力。  

　　性能最优类：负载均衡系统根据服务器的响应时间来进行任务分配，优先将新任务分配给响应最快的服务器。  

　　Hash 类：负载均衡系统根据任务中的某些关键信息进行 Hash 运算，将相同 Hash 值的请求分配到同一台服务器上。常见的有源地址 Hash、目标地址 Hash、session id hash、用户 ID Hash 等。  

#### **【轮询】**
　　负载均衡系统收到请求后，按照顺序轮流分配到服务器上。  

　　轮询是最简单的一个策略，无须关注服务器本身的状态。  

　　<b>只要服务器在运行，运行状态是不关注的。</b>但如果服务器直接宕机了，或者服务器和负载均衡系统断连了，这时负载均衡系统是能够感知的，也需要做出相应的处理。  

#### **【加权轮询】**
　　负载均衡系统根据服务器权重进行任务分配，这里的权重一般是根据硬件配置进行静态配置的，采用动态的方式计算会更加契合业务，但复杂度也会更高。  

　　加权轮询是轮询的一种特殊形式，其主要目的就是为了解决不同服务器处理能力有差异的问题。  

　　加权轮询解决了轮询算法中无法根据服务器的配置差异进行任务分配的问题，但同样存在无法根据服务器的状态差异进行任务分配的问题。  

#### **【负载最低优先】**
　　负载均衡系统将任务分配给当前负载最低的服务器，这里的负载根据不同的任务类型和业务场景，可以用不同的指标来衡量。  

　　例如：  

　　- LVS 这种 4 层网络负载均衡设备，可以以“连接数”来判断服务器的状态，服务器连接数越大，表明服务器压力越大。  
　　- Nginx 这种 7 层网络负载系统，可以以“HTTP 请求数”来判断服务器状态（Nginx 内置的负载均衡算法不支持这种方式，需要进行扩展）。  
　　- 如果我们自己开发负载均衡系统，可以根据业务特点来选择指标衡量系统压力。如果是 CPU 密集型，可以以“CPU 负载”来衡量系统压力；如果是 I/O 密集型，可以以“I/O 负载”来衡量系统压力。  

　　负载最低优先的算法解决了轮询算法中无法感知服务器状态的问题，由此带来的代价是复杂度要增加很多。  

　　例如：  

　　- 最少连接数优先的算法要求负载均衡系统统计每个服务器当前建立的连接，其应用场景仅限于负载均衡接收的任何连接请求都会转发给服务器进行处理，否则如果负载均衡系统和服务器之间是固定的连接池方式，就不适合采取这种算法。例如，LVS 可以采取这种算法进行负载均衡，而一个通过连接池的方式连接 MySQL 集群的负载均衡系统就不适合采取这种算法进行负载均衡。  

　　- CPU 负载最低优先的算法要求负载均衡系统以某种方式收集每个服务器的 CPU 负载，而且要确定是以 1 分钟的负载为标准，还是以 15 分钟的负载为标准，不存在 1 分钟肯定比 15 分钟要好或者差。不同业务最优的时间间隔是不一样的，时间间隔太短容易造成频繁波动，时间间隔太长又可能造成峰值来临时响应缓慢。  

　　负载最低优先算法如果本身没有设计好，或者不适合业务的运行特点，算法本身就可能成为性能的瓶颈，或者引发很多莫名其妙的问题。所以负载最低优先算法虽然效果看起来很美好，但实际上真正应用的场景反而没有轮询（包括加权轮询）那么多。  

#### **【性能最优类】**
　　负载最低优先类算法是站在服务器的角度来进行分配的，而性能最优优先类算法则是站在客户端的角度来进行分配的，优先将任务分配给处理速度最快的服务器，通过这种方式达到最快响应客户端的目的。  

　　和负载最低优先类算法类似，性能最优优先类算法本质上也是感知了服务器的状态，只是通过响应时间这个外部标准来衡量服务器状态而已。因此性能最优优先类算法存在的问题和负载最低优先类算法类似，复杂度都很高，主要体现在：  

　　- 负载均衡系统需要收集和分析每个服务器每个任务的响应时间，在大量任务处理的场景下，这种收集和统计本身也会消耗较多的性能。  

　　- 为了减少这种统计上的消耗，可以采取采样的方式来统计，即不统计所有任务的响应时间，而是抽样统计部分任务的响应时间来估算整体任务的响应时间。采样统计虽然能够减少性能消耗，但使得复杂度进一步上升，因为要确定合适的<b>采样率</b>，采样率太低会导致结果不准确，采样率太高会导致性能消耗较大，找到合适的采样率也是一件复杂的事情。  

　　- 无论是全部统计还是采样统计，都需要选择合适的周期：是 10 秒内性能最优，还是 1 分钟内性能最优，还是 5 分钟内性能最优……没有放之四海而皆准的周期，需要根据实际业务进行判断和选择，这也是一件比较复杂的事情，甚至出现系统上线后需要不断地调优才能达到最优设计。  

#### **【Hash 类】**
　　负载均衡系统根据任务中的某些关键信息进行 Hash 运算，将相同 Hash 值的请求分配到同一台服务器上，这样做的目的主要是为了满足特定的业务需求。  

　　例如：  

　　- 源地址 Hash：将来源于同一个源 IP 地址的任务分配给同一个服务器进行处理，适合于存在事务、会话的业务。  

　　- ID Hash：将某个 ID 标识的业务分配到同一个服务器中进行处理，这里的 ID 一般是临时性数据的 ID（如 session id）。  

#### **【脑图】**
![hello](/ARTS2021/ARTS-42/1.png)  

