---
layout: post
title: ARST(三十七)
date: 2020-12-28 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:152. Maximum Product Subarray】**

##### 　　Description:
　　Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.  

##### 　　Example 1:
```sh
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

##### 　　Example 2:
```sh
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/152.MaximumProductSubarray/MaximumProductSubarray.cpp  

###  <font color=yellow>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/c-20-concurrency  
　　C++20: Concurrency  

#### **【Words】**
English | Chinese
-|-
exceed  | vt. 超过；胜过
Latch | n. 门闩
intuitive | adj. 直觉的；凭直觉获知的

#### **【Comments and Conclusions】**

##### Overview of C++20 Concurrency
![hello](/ARTS2020/ARTS-37/1.png)

##### std::atomic_ref<T>
　　The class template std::atomic_ref applies atomic operations to the referenced non-atomic object. Concurrent writing and reading of the referenced object is, therefore, no data race. The lifetime of the referenced object must exceed the lifetime of the atomic_ref. Accessing a subobject of the referenced object with an atomic_ref is not thread-safe.  

　　Accordingly to std::atomic, std::atomic_ref can be specialised and supports specialisations for the built-in data types.  
```c++
struct Counters {
    int a;
    int b;
};

Counter counter;

std::atomic_ref<Counters> cnt(counter);
```

##### std::atomic<std::shared_ptr<T>> and std::atomic<std::weak_ptr<T>>
　　std::shared_ptr is the only non-atomic data type on which you can apply atomic operations.  

　　The C++ committee saw the necessity that instances of std::shared_ptr should provide a minimum atomicity guarantee in multithreading programs. What is this minimal atomicity guarantee for std::shared_ptr? The control block of the std::shared_ptr is thread-safe. This means that the increase and decrease operations of the reference-counter are atomic. You also have the guarantee that the resource is destroyed exactly once.  

　　The assertions that a std::shared_ptr provides, are described by Boost.  

　　- A shared_ptr instance can be “read” (accessed using only constant operations) simultaneously by multiple threads.  
　　- Different shared_ptr instances can be “written to” (accessed using mutable operations such as operator= or reset) simultaneously by multiple threads (even when these instances are copies, and share the same reference count underneath).  

##### Floating Point Atomics
　　In addition to C++11, you can not only create atomics for integral types but also for floating points. This is quite convenient when you have a floating-point number, which is concurrently incremented by various threads. With a floating-point atomic, you don't need to protect the floating pointer number.  

##### Waiting on Atomics
　　std::atomic_flag is an atomic boolean. It has a clear and a set state. For simplicity reasons, I call the clear state false and the set state true. Its clear method enables you to set its value to false. With the test_and_set method, you can set the value to true and return the previous value. There is no method to ask for the current value. This will change with C++20. With C++20, a  std::atomic_flag has a test method.  

　　Additionally, std::atomic_flag can be used for thread synchronisation via the methods notify_one, notify_all, and wait. Notifying and waiting is with C++20 available on all partial and full specialisation of std::atomic (bools, integrals, floats and pointers) and std::atomic_ref.  

#### Semaphores, Latches and Barriers
　　All three types are means to synchronise threads.  

　　1. Semaphores  
　　Semaphores are a synchronisation mechanism used to control concurrent access to a shared resource. A counting semaphore such as the on in C++20, is a special semaphore which has a counter that is bigger than zero. The counter is initialised in the constructor. Acquiring the semaphore decreases the counter and releasing the semaphore increases the counter. If a thread tries to acquire the semaphore when the counter is zero, the thread will block until another thread increments the counter by releasing the semaphore.  

　　2. Latches and Barriers  
　　Latches and barriers are simple thread synchronisation mechanisms which enable some threads to block until a counter becomes zero.  

　　What are the differences between these two mechanisms to synchronise threads? You can use a std::latch only once, but you can use a std::barrier more than once. A std::latch is useful for managing one task by multiple threads; a std::barrier is useful for managing repeated tasks by multiple threads. Additionally, a std::barrier can adjust the counter in each iteration.  

　　Here is the following code snippet:  
```c++
void DoWork(threadpool* pool) {
    latch completion_latch(NTASKS);
    for (int i = 0; i < NTASKS; ++i) {
        pool->add_task([&] {
            // perform work
            completion_latch.count_down();
        });
    }
    // Block until work is done
    completion_latch.wait();  // (5)
}
```

#### std::jthread
　　std::jthread stands for joining thread. In addition to std::thread from C++11, std::jthread can automatically join the started thread and can be interrupted.  

　　the thread thr automatically joins in its destructor such as in this case if still joinable.  
```c++
#include <iostream>
#include <thread>

int main() {
    std::cout << std::boolalpha;
    std::jthread thr{[] { std::cout << "Joinable std::thread" << std::endl; }};
    std::cout << "thr.joinable(): " << thr.joinable() << std::endl;
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// thr.joinable(): true
// Joinable std::thread
```

###  <font color=yellow>tips: 如何高效阅读源代码？</font>
https://www.bilibili.com/video/BV1ke411W7r7  

##### 高效阅读方法
　　1. 自顶向下：优点是对源码有个全局观，对软件的模块功能有个全面的了解。  

　　2. 自底往上：优点是对软件功能代码的实现细节非常熟悉。  

　　3. Mixed：推荐的方法。  

###  <font color=yellow>Share: 高性能NoSQL</font>

#### **【关系数据库缺点】**

##### 1. 关系数据库存储的是行记录，无法存储数据结构

##### 2. 关系数据库的 schema 扩展很不方便
　　关系数据库的表结构 schema 是强约束，操作不存在的列会报错，业务变化时扩充列也比较麻烦，需要执行 DDL（data definition language，如 CREATE、ALTER、DROP 等）语句修改，而且修改时可能会长时间锁表。  

##### 3. 关系数据库在大数据场景下 I/O 较高
　　如果对一些大量数据的表进行统计之类的运算，关系数据库的 I/O 会很高。  

##### 4. 关系数据库的全文搜索功能比较弱
　　关系数据库的全文搜索只能使用 like 进行整表扫描匹配，性能非常低。  

　　针对上述问题，分别诞生了不同的 NoSQL 解决方案，这些方案与关系数据库相比，在某些应用场景下表现更好。但世上没有免费的午餐，NoSQL 方案带来的优势，本质上是牺牲 ACID 中的某个或者某几个特性。  

#### **【K-V 存储】**
　　解决关系数据库无法存储数据结构的问题，以 Redis 为代表。  

　　Redis 是 K-V 存储的典型代表，它是一款开源（基于 BSD 许可）的高性能 K-V 缓存和存储系统。Redis 的 Value 是具体的数据结构，包括 string、hash、list、set、sorted set、bitmap 和 hyperloglog，所以常常被称为数据结构服务器。  

　　Redis 的缺点主要体现在并不支持完整的 ACID 事务，Redis 虽然提供事务功能，但 Redis 的事务和关系数据库的事务不可同日而语，Redis 的事务只能保证隔离性和一致性（I 和 C），无法保证原子性和持久性（A 和 D）。  

#### **【文档数据库】**
　　解决关系数据库强 schema 约束的问题，以 MongoDB 为代表。  

　　文档数据库最大的特点就是 no-schema，可以存储和读取任意的数据。目前绝大部分文档数据库存储的数据格式是 JSON（或者 BSON）。  

　　文档数据库的 no-schema 特性，给业务开发带来了几个明显的优势。  

##### 1. 新增字段简单

##### 2. 历史数据不会出错

##### 3. 可以很容易存储复杂数据
　　文档数据库的这个特点，特别适合电商和游戏这类的业务场景。  

　　文档数据库 no-schema 的特性带来的这些优势也是有代价的，最主要的代价就是不支持事务。  

#### **【列式数据库】**
　　解决关系数据库大数据场景下的 I/O 问题，以 HBase 为代表。  

　　列式数据库就是按照列来存储数据的数据库，与之对应的传统关系数据库被称为“行式数据库”，因为关系数据库是按照行来存储数据的。  

　　除了节省 I/O，列式存储还具备更高的存储压缩比，能够节省更多的存储空间。普通的行式数据库一般压缩率在 3:1 到 5:1 左右，而列式数据库的压缩率一般在 8:1 到 30:1 左右。  

　　缺点：列式存储将不同列存储在磁盘上不连续的空间，导致更新多个列时磁盘是随机写操作。  

　　基于上述列式存储的优缺点，一般将列式存储应用在离线的大数据分析和统计场景中，因为这种场景主要是针对部分列单列进行操作，且数据写入后就无须再更新删除。  

#### **【全文搜索引擎】**
　　在全文搜索的业务场景下，索引也无能为力，主要体现在：  

　　1. 全文搜索的条件可以随意排列组合，如果通过索引来满足，则索引的数量会非常多。  
　　2. 全文搜索的模糊匹配方式，索引无法满足，只能用 like 查询，而 like 查询是整表扫描，效率非常低。  

##### 1. 全文搜索基本原理
　　全文搜索引擎的技术原理被称为“倒排索引”（Inverted index），也常被称为反向索引、置入档案或反向档案，是一种索引方法，其基本原理是建立单词到文档的索引。之所以被称为“倒排”索引，是和“正排“索引相对的，“正排索引”的基本原理是建立文档到单词的索引。  

##### 2. 全文搜索的使用方式
　　全文搜索引擎的索引对象是单词和文档，而关系数据库的索引对象是键和行，两者的术语差异很大，不能简单地等同起来。因此，为了让全文搜索引擎支持关系型数据的全文搜索，需要做一些转换操作，即将关系型数据转换为文档数据。  

　　目前常用的转换方式是将关系型数据按照对象的形式转换为 JSON 文档，然后将 JSON 文档输入全文搜索引擎进行索引。  

#### **【脑图】**
![hello](/ARTS2020/ARTS-37/2.png)


