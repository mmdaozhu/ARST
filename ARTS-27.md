---
layout: post
title: ARST(二十七)
date: 2020-10-19 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=purple>Algorithm</font>

#### **【LeetCode:24. Swap Nodes in Pairs】**

##### 　　Description:
　　Given a linked list, swap every two adjacent nodes and return its head.  

　　You may not modify the values in the list's nodes. Only nodes itself may be changed.  

##### 　　Example 1:
```sh
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

##### 　　Example 2:
```sh
Input: head = []
Output: []
```

##### 　　Example 3:
```sha
Input: head = [1]
Output: [1]
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/024.SwapNodesInPairs/SwapNodesInPairs.cpp  

###  <font color=purple>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
skew | v. 偏离，歪斜；扭转
uneven | adj. 不均匀的；不平坦的；
spike | vt. 阻止
Tweak | v. 扭
invalidate | vt. 使无效；使无价值

#### **【Comments and Conclusions】**

##### Cache
　　Caching improves page load times and can reduce the load on your servers and databases. In this model, the dispatcher will first lookup if the request has been made before and try to find the previous result to return, in order to save the actual execution.  

　　Databases often benefit from a uniform distribution of reads and writes across its partitions. Popular items can skew the distribution, causing bottlenecks. Putting a cache in front of a database can help absorb uneven loads and spikes in traffic.  

##### Client caching
　　Caches can be located on the client side (OS or browser), server side, or in a distinct cache layer.  

##### CDN caching
　　CDNs are considered a type of cache.  

##### Web server caching
　　Reverse proxies and caches such as Varnish can serve static and dynamic content directly. Web servers can also cache requests, returning responses without having to contact application servers.  

##### Database caching
　　Your database usually includes some level of caching in a default configuration, optimized for a generic use case. Tweaking these settings for specific usage patterns can further boost performance.  

##### Application caching
　　In-memory caches such as Memcached and Redis are key-value stores between your application and your data storage. Since the data is held in RAM, it is much faster than typical databases where data is stored on disk. RAM is more limited than disk, so cache invalidation algorithms such as least recently used (LRU) can help invalidate 'cold' entries and keep 'hot' data in RAM.  

###  <font color=purple>tips: 前Google工程师的算法学习与面试经验分享</font>
https://www.bilibili.com/video/BV1j7411i7mR  

##### 用于工作
　　1. 基础的数据结构和算法  
　　基础的数据结构：链表、数组、队列、栈、二叉树、堆、图的定义等  

　　基础的算法：排序、二分查找、二叉树的遍历、图的广度、深度优先搜索、字符串朴素匹配算法  

　　基础的算法思想：递归、分治、贪心、回溯、动态规划  

　　2. 高级的数据结构和算法  
　　红黑树、BM算法、KMP算法、AC自动机等，还有图的一些算法  

##### 应付面试
　　刷leetcode  

##### 算法面试到底考察候选人什么
　　1. 逻辑思维能力  
　　2. 编写复杂代码的能力  
　　3. 基础数据结构和算法的掌握  
　　4. 时间、空间复杂度分析能力  
　　5. 编写bug free代码的能力  
　　6. 代码是否整洁、是否符合编码规范  

##### 应对算法面试的一些小技巧
　　1. 多搜面经、知己知彼  
　　2. 练习白板编程  
　　3. 尽量保证代码没有bug  
　　4. 尽量保证代码规范  
　　5. 要有时间意识  
　　6. 先用最简单的方法解决  

###  <font color=purple>Share: 深入剖析单例模式（二）</font>
　　在上一节中我讲解了什么是单例模式，单例模式的应用场景以及一个简单的实现。  

　　在这一节中我们继续来看单例模式的另外几种实现方式。  

#### **【双重检测】**
　　为了保证Lazy初始化的线程安全性，又要保证性能问题，于是就出现了双重检测的单例模式。  

　　在这个例子中，如果Singleton对象已经创建出来了，就不出现进入加锁逻辑了。这种方式解决了并发度低的问题。  

　　具体看如下代码：  
```c++
#include <iostream>
#include <memory>
#include <mutex>
#include <thread>

class Singleton {
public:
    static std::shared_ptr<Singleton> Instance();
    ~Singleton() { std::cout << "~Singleton()" << std::endl; }

protected:
    Singleton() { std::cout << "Singleton()" << std::endl; }

private:
    static std::shared_ptr<Singleton> instance_;
    static std::mutex mtx_;
};

std::shared_ptr<Singleton> Singleton::instance_;
std::mutex Singleton::mtx_;

std::shared_ptr<Singleton> Singleton::Instance() {
    if (instance_.get() == nullptr) {
        std::lock_guard<std::mutex> lck(mtx_);
        if (instance_.get() == nullptr) {
            instance_ = std::shared_ptr<Singleton>(new Singleton());
        }
    }
    return instance_;
}

void foo() { Singleton::Instance(); }

void bar() { Singleton::Instance(); }

int main() {
    std::thread t1(foo);
    std::thread t2(bar);
    std::cout << "main, foo and bar now execute concurrently...\n";
    t1.join();
    t2.join();
    std::cout << "foo and bar completed.\n";
    return 0;
}
// g++ -std=c++11 test.cpp -o test -lpthread
// result:
// main, foo and bar now execute concurrently...
// Singleton()
// foo and bar completed.
// ~Singleton()
```
　　但这种方式有个线程安全的隐患。因为指令重排序问题，有可能在创建Singleton时，等内存分配出来之后，instance_所持有的指针直接指向分配出来的内存，但是Singleton并没有构造出来。  

　　如果用volatile修饰符和内存栅栏等方法，会变得超级复杂。具体参考Scott Meyers and Andrei Alexandrescu的《C++ and the Perils of Double-Checked Locking》论文。  

　　所以我并不推荐用双重检测的方式，大家了解一下就行。  

#### **【Meyers的单例】**
　　接下来是Meyers的单例，这是最简单的单例模式，既支持Lazy初始化，又保证线程安全和性能，它不需要锁机制。  

　　因为c++11规定了在多线程环境下，一个线程在local static对象初始化到结束期间，其他线程都会等待，直到local static对象初始化完成。这样就保证了线程安全性。  

　　具体看如下代码：  
```c++
#include <iostream>
#include <thread>

class Singleton {
public:
    static Singleton& Instance();
    ~Singleton() { std::cout << "~Singleton()" << std::endl; }

protected:
    Singleton() { std::cout << "Singleton()" << std::endl; }
};

Singleton& Singleton::Instance() {
    static Singleton instance_;
    return instance_;
}

void foo() { Singleton::Instance(); }

void bar() { Singleton::Instance(); }

int main() {
    std::thread t1(foo);
    std::thread t2(bar);
    std::cout << "main, foo and bar now execute concurrently...\n";
    t1.join();
    t2.join();
    std::cout << "foo and bar completed.\n";
    return 0;
}
// g++ -std=c++11 test.cpp -o test -lpthread
// result:
// main, foo and bar now execute concurrently...
// Singleton()
// foo and bar completed.
// ~Singleton()
```

#### **【饿汉式】**
　　在这种单例模式下，单例在程序开始之前就已经初始化好了，不存在多个线程竞争的问题。缺点就是会影响程序启动速度。  

　　示例代码如下：  
```c++
#include <iostream>
#include <memory>

class Singleton {
public:
    static std::shared_ptr<Singleton> Instance();
    ~Singleton() { std::cout << "~Singleton()" << std::endl; }

protected:
    Singleton() { std::cout << "Singleton()" << std::endl; }

private:
    static std::shared_ptr<Singleton> instance_;
};

std::shared_ptr<Singleton> Singleton::instance_ = std::shared_ptr<Singleton>(new Singleton());

std::shared_ptr<Singleton> Singleton::Instance() { return instance_; }

int main() {
    std::cout << "main start.\n";
    Singleton::Instance();
    Singleton::Instance();
    std::cout << "main end.\n";
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// Singleton()
// main start.
// main end.
// ~Singleton()
```
　　在这段代码中，关键在于instance_成员变量初始化的时候就创建单例对象。它有点类似于全局静态变量，在main之前系统就帮他初始化好。  

　　以上就是我要介绍的单例模式的几种实现方式：懒汉式，双重检测，Meyers的单例以及饿汉式。  

　　当然单例模式的实现还有很多种，关键在于适用什么样的应用场景。比如一个全局配置类，需要加载应用的配置信息，其实我们没必要设计成线程安全的单例，只需保证线程开启前，初始化好即可。  

　　单例模式我们遵循简单易用即可。  

　　