---
layout: post
title: ARST(四十)
date: 2021-01-18 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:188. Best Time to Buy and Sell Stock IV】**

##### 　　Description:
　　You are given an integer array prices where prices[i] is the price of a given stock on the ith day, and an integer k.  

　　Find the maximum profit you can achieve. You may complete at most k transactions.  

　　Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).  

##### 　　Example 1:
```sh
Input: k = 2, prices = [2,4,1]
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```

##### 　　Example 2:
```sh
Input: k = 2, prices = [3,2,6,5,0,3]
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/188.BestTimeToBuyAndSellStockIV/BestTimeToBuyAndSellStockIV.cpp  

###  <font color=yellow>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/brief-overview-of-the-pvs-studio-static-code-analyzer  
　　A Brief Overview of the PVS-Studio Static Code Analyzer  

#### **【Words】**
English | Chinese
-|-
stringent | adj. 严格的；严厉的
pivotal | adj. 关键的；中枢的；枢轴的
contrived | adj. 人为的；做作的；不自然的
auxiliary | adj. 辅助的

#### **【Comments and Conclusions】**
　　PVS-Studio is a static code analyzer, it integrates with variou third-party solutions and has interesting auxiliary subsystems.   

　　The teams runs the analyzer on a large codebase and gets many warnings. If the project is alive, then the critical bugs have somehow been correced in more expensive ways.  

　　If you want to download this tool, please refer to the https://www.viva64.com/en/pvs-studio/  

###  <font color=yellow>tips: c++面试（一）</font>

#### **【c++笔试题】**
　　1. 若char是一字节，int是4字节，指针类型是4字节，代码如下：  
```c++
class CTest {
public:
    CTest() : m_chData('\0'), m_nData(0) {}
    virtual void mem_fun() {}

private:
    char m_chData;
    int m_nData;
    static char s_chData;
};
char CTest::s_chData = '\0';
```

　　问：
　　（1）若按4字节对齐sizeof(CTest)的值是多少？  
　　（2）若按1字节对齐sizeof(CTest)的值是多少？  

　　答案：(1)虚函数表指针4+4+4=12. (2)虚函数表指针4+1+4=9.  


　　2. 下面程序的输出结果是（）  
```c++
#include <stdio.h>

int main() {
    int i, n = 0;
    float x = 1, y1 = 2.1 / 1.9, y2 = 1.9 / 2.1;
    for (i = 1; i < 22; i++) x = x * y1;
    while (x != 1.0) {
        x = x * y2;
        n++;
    }
    double x = 28;
    int r = x % 5;
    printf("%d\n", n);
    return 0;
}
```
　　A. 21  
　　B. 22  
　　C. 程序无线循环  
　　D. 运行时崩溃  

　　答案：C. 浮点数不能精确相等。  

　　3. 请问下面的程序一共输出多少个“-”？  
```c++
int main(void) {
    int i;
    for (i = 0; i < 2; i++) {
        fork();
        printf("-");
    }
    return 0;
}
```
　　A. 2  
　　B. 4  
　　C. 6  
　　D. 8  

　　答案：D. http://coolshell.cn/articles/7965.html  

#### **【c++面试题】**

##### 1. 说一说c++中四种cast转换？  

　　回答：c++中的四种cast转换有static_cast、dynamic_cast、const_cast、reinterpret_cast。  

　　1. static_cast  
　　static_cast相当于c语言中的强制转换，一般用于基本数据类型之间的转换，如把int转换成char，把double转换成int等等。还可以用于类层次结构中基类和派生类之间指针或者引用的转换。向上转换是安全的，向下转换是不安全的。  

　　2. dynamic_cast  
　　dynamic_cast用于动态类型的转换。用于含有虚函数的类层次的向上转换和向下转换。两种转换都是安全的，对于指针向下转换失败，则返回NULL。对于引用则抛异常。  

　　3. const_cast  
　　const_cast用于将const变量转换为non-const。  

　　4. reinterpret_cast  
　　reinterpret_cast用于执行低级转型，转换的对象一般为指针、引用、算术类型等。安全性差。  

##### 2. 请你来说一下C++中的智能指针
　　c++的智能指针是用于管理内存资源，防止内存泄漏。通过智能指针的构造函数来申请资源，析构函数来释放资源。  

　　c++中有四种智能指针：auto_ptr、unique_ptr、shared_ptr、weak_ptr。  

　　1. auto_ptr(cpp11已经废弃)  
　　auto_ptr是不安全的，比如一不小心将一个auto_ptr p1赋值给一个auto_ptr p2。因为p2剥夺了p1的所有权，程序再次访问p1时，则会导致内存崩溃。  

　　2. unique_ptr(替换auto_ptr)  
　　unique_ptr是auto_ptr的改良，比auto_ptr安全。如果p1=p2，则会编译失败。程序员必须这样写p1=std::move(p2)，传递一个右值引用给p1。  

　　3. shared_ptr  
　　shared_ptr指多个智能指针可以指向相同的对象，它使用计数机制实现资源的共享。当一个智能指针复制时，则引用计数会加一；当一个智能指针销毁时，则引用计数会减一。当所有的智能指针销毁时，则共享资源会被释放。  

　　4 . weak_ptr  
　　weak_ptr是不控制对象生命周期的智能指针，它指向一个由shared_ptr管理的对象。它的构造和析构不会引用计数的增加或者减少。weak_ptr是用来解决shared_ptr循环引用时导致的死锁问题。  


#### **【操作系统面试题】**

##### 1. 请你说一说进程和线程的区别
　　进程是操作系统分配资源的最小单位，线程是操作系统调度的最小单位。  

　　进程有独立的系统资源，而同一个进程内的线程共享进程中的大部分系统资源，包括堆、代码段、数据段等等。  

　　一个线程只能属于一个进程，而一个进程可以有多个线程。  

　　进程在创建、切换和销毁时开销比较大，而线程比较小。进程在创建和销毁时，系统要为之分配或回收资源，例如系统内存、IO设备，操作系统所付出的开销要远远大于线程创建和销毁的开销。进程在切换的时候需要虚拟地址空间的切换、CPU环境的保存等，而线程不需要。  

　　进程间通信比较复杂，而同一个进程的线程由于共享代码段和数据段，通信比较简单。  

　　一个进程崩溃，不会影响其他进程；一个线程崩溃，其所属进程的其他线程都会崩溃。  

##### 2. 请问进程间是怎么通信的
　　进程间的通信有管道、信号、消息队列、信号量、共享内存、socket等等。  

　　1.管道：管道主要包括无名管道和命令管道。管道是半双工的，具有固定的读端和写端。它可以看成一种特殊的文件，对它的读写也可以使用普通的read、write等函数。但是它不属于普通的文件，不属于任何文件系统，并且只存在内存中。但是命令管道是以一种特殊设备文件形式存在于文件系统中。  

　　2. 信号：信号是一种比较复杂的通信方式，用于通知接受进程某个事件发生。  

　　3. 消息队列：消息队列是消息的链表，存放在内核中。它是面向记录的，独立于发送与接受进程。  

　　4. 信号量：信号量是一个计数器，用于控制进程间的同步与互斥。  

　　5. 共享内存：共享内存使得多个进程可以访问同一块内存空间。  

　　6. socket：socket是端对端的进程通信方式，有本地和远程两种。  

#### **【计算机网络面试题】**

##### 1. 请详细介绍一个TCP的三次握手机制，为什么要三次握手？
　　两个点：为什么需要握手？为什么需要三次？

　　TCP保证了数据传输的可靠性，是双工的。客户端和服务端是可以同时发消息的，这时每次发消息都必须有个seq，这样对方才能确定消息是收到的。（为什么需要握手）TCP建立的时候不可以是半打开状态，半建立状态就开始传输消息，所以服务端在回ACK的时候，必须将它的seq发过来。  

##### 2. 介绍下CLOSE_WAIT状态产生的原因
　　主动端发送FIN给被动端，被动端接收到FIN之后，发送一个ACK给主动端。这时被动端处于CLOSE_WAIT状态，被动端仍然可以向主动端发送消息。  

###  <font color=yellow>Share: 单服务器高性能模式：Reactor与Proactor</font>

#### **【Reactor】**
　　I/O 多路复用技术归纳起来有两个关键实现点：  

　　- 当多条连接共用一个阻塞对象后，进程只需要在一个阻塞对象上等待，而无须再轮询所有连接，常见的实现方式有 select、epoll、kqueue 等。  
　　- 当某条连接有新的数据可以处理时，操作系统会通知进程，进程从阻塞状态返回，开始进行业务处理。  

　　I/O 多路复用结合线程池，完美地解决了 PPC 和 TPC 的问题，而且“大神们”给它取了一个很牛的名字：Reactor。  

　　Reactor 模式也叫 Dispatcher 模式，更加贴近模式本身的含义，即 I/O 多路复用统一监听事件，收到事件后分配（Dispatch）给某个进程。  

　　Reactor 模式的核心组成部分包括 Reactor 和处理资源池（进程池或线程池），其中 Reactor 负责监听和分配事件，处理资源池负责处理事件。初看 Reactor 的实现是比较简单的，但实际上结合不同的业务场景，Reactor 模式的具体实现方案灵活多变，主要体现在：  

　　- Reactor 的数量可以变化：可以是一个 Reactor，也可以是多个 Reactor。  
　　- 资源池的数量可以变化：以进程为例，可以是单个进程，也可以是多个进程（线程类似）。  

　　Reactor 模式有这三种典型的实现方案：  

　　- 单 Reactor 单进程 / 线程。  
　　- 单 Reactor 多线程。  
　　- 多 Reactor 多进程 / 线程。  

　　以上方案具体选择进程还是线程，更多地是和编程语言及平台相关。例如，Java 语言一般使用线程（例如，Netty），C 语言使用进程和线程都可以。例如，Nginx 使用进程，Memcache 使用线程。  

##### 1. 单 Reactor 单进程 / 线程

　　单 Reactor 单进程 / 线程的方案示意图如下（以进程为例）：  
![hello](/ARTS2021/ARTS-40/1.jpg)  

　　详细说明一下这个方案：  

　　- Reactor 对象通过 select 监控连接事件，收到事件后通过 dispatch 进行分发。  
　　- 如果是连接建立的事件，则由 Acceptor 处理，Acceptor 通过 accept 接受连接，并创建一个 Handler 来处理连接后续的各种事件。  
　　- 如果不是连接建立事件，则 Reactor 会调用连接对应的 Handler（第 2 步中创建的 Handler）来进行响应。  
　　- Handler 会完成 read-> 业务处理 ->send 的完整业务流程。  

　　单 Reactor 单进程的模式优点就是很简单，没有进程间通信，没有进程竞争，全部都在同一个进程内完成。但其缺点也是非常明显，具体表现有：  

　　- 只有一个进程，无法发挥多核 CPU 的性能；只能采取部署多个系统来利用多核 CPU，但这样会带来运维复杂度，本来只要维护一个系统，用这种方式需要在一台机器上维护多套系统。  
　　- Handler 在处理某个连接上的业务时，整个进程无法处理其他连接的事件，很容易导致性能瓶颈。  

　　因此，单 Reactor 单进程的方案在实践中应用场景不多，<b>只适用于业务处理非常快速的场景</b>，目前比较著名的开源软件中使用单 Reactor 单进程的是 Redis。  

##### 2. 单 Reactor 多线程

　　单 Reactor 多线程方案示意图是：  
![hello](/ARTS2021/ARTS-40/2.jpg)  

　　我来介绍一下这个方案：  

　　- 主线程中，Reactor 对象通过 select 监控连接事件，收到事件后通过 dispatch 进行分发。  
　　- 如果是连接建立的事件，则由 Acceptor 处理，Acceptor 通过 accept 接受连接，并创建一个 Handler 来处理连接后续的各种事件。  
　　- 如果不是连接建立事件，则 Reactor 会调用连接对应的 Handler（第 2 步中创建的 Handler）来进行响应。  
　　- Handler 只负责响应事件，不进行业务处理；Handler 通过 read 读取到数据后，会发给 Processor 进行业务处理。  
　　- Processor 会在独立的子线程中完成真正的业务处理，然后将响应结果发给主进程的 Handler 处理；Handler 收到响应后通过 send 将响应结果返回给 client。  

　　单 Reator 多线程方案能够充分利用多核多 CPU 的处理能力，但同时也存在下面的问题：  

　　- 多线程数据共享和访问比较复杂。例如，子线程完成业务处理后，要把结果传递给主线程的 Reactor 进行发送，这里涉及共享数据的互斥和保护机制。以 Java 的 NIO 为例，Selector 是线程安全的，但是通过 Selector.selectKeys() 返回的键的集合是非线程安全的，对 selected keys 的处理必须单线程处理或者采取同步措施进行保护。  
　　- Reactor 承担所有事件的监听和响应，只在主线程中运行，瞬间高并发时会成为性能瓶颈。  

##### 2. 多 Reactor 多进程 / 线程
　　多 Reactor 多进程 / 线程方案示意图是（以进程为例）：  
![hello](/ARTS2021/ARTS-40/3.jpg)  

　　方案详细说明如下：  

　　- 父进程中 mainReactor 对象通过 select 监控连接建立事件，收到事件后通过 Acceptor 接收，将新的连接分配给某个子进程。  
　　- 子进程的 subReactor 将 mainReactor 分配的连接加入连接队列进行监听，并创建一个 Handler 用于处理连接的各种事件。  
　　- 当有新的事件发生时，subReactor 会调用连接对应的 Handler（即第 2 步中创建的 Handler）来进行响应。  
　　- Handler 完成 read→业务处理→send 的完整业务流程。  

　　多 Reactor 多进程 / 线程的方案看起来比单 Reactor 多线程要复杂，但实际实现时反而更加简单，主要原因是：  

　　- 父进程和子进程的职责非常明确，父进程只负责接收新连接，子进程负责完成后续的业务处理。  
　　- 父进程和子进程的交互很简单，父进程只需要把新连接传给子进程，子进程无须返回数据。  
　　- 子进程之间是互相独立的，无须同步共享之类的处理（这里仅限于网络模型相关的 select、read、send 等无须同步共享，“业务处理”还是有可能需要同步共享的）。  

#### **【Proactor】**
　　Reactor 是非阻塞同步网络模型，因为真正的 read 和 send 操作都需要用户进程同步操作。这里的“同步”指用户进程在执行 read 和 send 这类 I/O 操作的时候是同步的，如果把 I/O 操作改为异步就能够进一步提升性能，这就是异步网络模型 Proactor。  

　　<b>Reactor 可以理解为“来了事件我通知你，你来处理”，而 Proactor 可以理解为“来了事件我来处理，处理完了我通知你”。</b>这里的“我”就是操作系统内核，“事件”就是有新连接、有数据可读、有数据可写的这些 I/O 事件，“你”就是我们的程序代码。  

　　Proactor 模型示意图是：  
![hello](/ARTS2021/ARTS-40/4.jpg)  

　　详细介绍一下 Proactor 方案：  

　　- Proactor Initiator 负责创建 Proactor 和 Handler，并将 Proactor 和 Handler 都通过 Asynchronous Operation Processor 注册到内核。  
　　- Asynchronous Operation Processor 负责处理注册请求，并完成 I/O 操作。  
　　- Asynchronous Operation Processor 完成 I/O 操作后通知 Proactor。  
　　- Proactor 根据不同的事件类型回调不同的 Handler 进行业务处理。  
　　- Handler 完成业务处理，Handler 也可以注册新的 Handler 到内核进程。  

　　理论上 Proactor 比 Reactor 效率要高一些，异步 I/O 能够充分利用 DMA 特性，让 I/O 操作与计算重叠，但要实现真正的异步 I/O，操作系统需要做大量的工作。目前 Windows 下通过 IOCP 实现了真正的异步 I/O，而在 Linux 系统下的 AIO 并不完善，因此在 Linux 下实现高并发网络编程时都是以 Reactor 模式为主。所以即使 Boost.Asio 号称实现了 Proactor 模型，其实它在 Windows 下采用 IOCP，而在 Linux 下是用 Reactor 模式（采用 epoll）模拟出来的异步模型。  

#### **【脑图】**
![hello](/ARTS2021/ARTS-40/5.png)  

#### **【实现】**
https://github.com/mmdaozhu/high-performance-server-model  


