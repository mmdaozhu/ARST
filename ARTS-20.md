---
layout: post
title: ARST(二十)
date: 2020-08-31 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=blue>Algorithm</font>

#### **【LeetCode:322. Coin Change】**
　　You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.  

　　You may assume that you have an infinite number of each kind of coin.  

##### 　　Example 1:
```sh
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

##### 　　Example 2:
```sh
Input: coins = [2], amount = 3
Output: -1
```

##### 　　Example 3:
```sh
Input: coins = [1], amount = 0
Output: 0
```

##### 　　Example 4:
```sh
Input: coins = [1], amount = 1
Output: 1
```

##### 　　Example 5:
```sh
Input: coins = [1], amount = 2
Output: 2
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/322.CoinChange/CoinChange.cpp  

###  <font color=blue>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
commodity | n. 商品，货物；日用品

#### **【Comments and Conclusions】**

##### Load balancer
　　Load balancers distribute incoming client requests to computing resources such as application servers and databases. In each case, the load balancer returns the response from the computing resource to the appropriate client. Load balancers are effective at:  

　　- Preventing requests from going to unhealthy servers  
　　- Preventing overloading resources  
　　- Helping to eliminate a single point of failure  

　　Load balancers can be implemented with hardware (expensive) or with software such as HAProxy.  

　　To protect against failures, it's common to set up multiple load balancers, either in active-passive or active-active mode.  

　　Load balancers can route traffic based on various metrics, including:  

　　- Random  
　　- Least loaded  
　　- Session/cookies  
　　- Round robin or weighted round robin  
　　- Layer 4  
　　- Layer 7  

##### Layer 4 load balancing
　　Layer 4 load balancers look at info at the transport layer to decide how to distribute requests. Generally, this involves the source, destination IP addresses, and ports in the header, but not the contents of the packet. Layer 4 load balancers forward network packets to and from the upstream server, performing Network Address Translation (NAT).  

##### Layer 7 load balancing
　　Layer 7 load balancers look at the application layer to decide how to distribute requests. This can involve contents of the header, message, and cookies. Layer 7 load balancers terminate network traffic, reads the message, makes a load-balancing decision, then opens a connection to the selected server.  

　　At the cost of flexibility, layer 4 load balancing requires less time and computing resources than Layer 7, although the performance impact can be minimal on modern commodity hardware.  

##### Horizontal scaling
　　Load balancers can also help with horizontal scaling, improving performance and availability. Scaling out using commodity machines is more cost efficient and results in higher availability than scaling up a single server on more expensive hardware, called Vertical Scaling.  

##### Disadvantage(s): load balancer

　　- The load balancer can become a performance bottleneck if it does not have enough resources or if it is not configured properly.  
　　- Introducing a load balancer to help eliminate a single point of failure results in increased complexity.  
　　- A single load balancer is a single point of failure, configuring multiple load balancers further increases complexity.  
　
###  <font color=blue>tips: 阿里云前高级技术专家陶辉：如何系统性构建网络协议知识体系，真正掌握 Web 协议？</font>

https://www.bilibili.com/video/BV1pJ411p7bS  

##### 1.OSI分层（应用层、表示层、会话层、传输层、网络层、数据链路层、物理层）
　　OSI分层对系统性能提升有指导作用。  

　　1.QPS、TPS、报文平均大小（应用层）  
　　2.握手、对称加密、TLS（表示层）  
　　3.网卡吞吐量、BPS传输指标、优化TPC握手（传输层）  
　　4.PPS传输指标（网络层）  
　　5.广播、ARP欺骗（数据链路层）  

##### 2.信息怎样才能高效的传输

　　1.编解码效率提升  
　　　　- 采用高效的编码算法，如哈夫曼编码（HTTP2）  
　　　　- 静态和动态表（HTTP2）  
　　　　- Frame（HTTP2）  

　　2.传输效率提升  
　　　　- 载荷比例（TCP延迟确认优化、Nagle算法）  
　　　　- 丢包（拥塞算法）  
　　　　- 重复、延迟  
　　　　- 路径  
　　　　- 噪音（校验和，自动纠错）  
　　　　- 多路复用  

##### 3.安全

　　1.传输安全  
　　　　- OTP  
　　　　- TLS（非对称加密，密钥协商算法，中间人）  
　　　　- 公钥认证体系（中间人）  

　　2.共享安全  
　　　　- 浏览器的同源策略  

##### 4.协程是怎么实现的
　　在进程中，如果一个协程执行不下去了，则把这个协程的IP、SP寄存器保存到内存中。然后将保存在内存中的另外一个协程的IP、SP寄存器的值取出，开始执行另外一个协程。  

　　如果协程被socket阻塞了，则把这个协程的有关网络的操作、协程锁封装保存起来。  

　　协程是一个复杂的框架。  

###  <font color=blue>Share: boost.statechat库下的状态模式</font>
　　状态模式一般用来实现状态机，而状态机常用在游戏、工作流引擎等系统开发中。  

　　有限状态机，英文翻译是 Finite State Machine，缩写为 FSM，简称为状态机。状态机有 3 个组成部分：状态（State）、事件（Event）、动作（Action）。其中，事件也称为转移条件（Transition Condition）。事件触发状态的转移及动作的执行。不过，动作不是必须的，也可能只转移状态，不执行任何动作。  

　　状态模式通过将事件触发的状态转移和动作执行，拆分到不同的状态类中，来避免分支判断逻辑。  

#### **【state_machine和simple_state类概要】**
```c++
template< class MostDerived,
          class InitialState,
          class Allocator = std::allocator< none >,
          class ExceptionTranslator = null_exception_translator >
class state_machine : noncopyable

template< class MostDerived,
          class Context,
          class InnerInitial = mpl::list<>,
          history_mode historyMode = has_no_history >
class simple_state : public detail::simple_state_base_type< MostDerived,
  typename Context::inner_context_type, InnerInitial >::type
```
　　可以看出，模板类state_machine的第一个模板参数是状态机，第二个模板参数是初始状态。  

　　相同的模板类simple_state的第一个模板参数是状态，第二个模板参数是它所属的状态机。  

#### **【简单的Hello World】**
　　直接上代码：
```c++
#include <boost/statechart/simple_state.hpp>
#include <boost/statechart/state_machine.hpp>
#include <iostream>

struct Greeting;

struct Machine : boost::statechart::state_machine<Machine, Greeting> {};

struct Greeting : boost::statechart::simple_state<Greeting, Machine> {
    Greeting() { std::cout << "Hello World!" << std::endl; }

    ~Greeting() { std::cout << "Bye Bye World!" << std::endl; }
};

int main() {
    Machine machine;
    machine.initiate();
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// Hello World!
// Bye Bye World!
```
　　在代码中我们定义初始状态为Greeting的状态机Machine，通过调用Machine的initiate成员函数来触发状态Greeting的构造函数；当Machine析构时，会导致Machine中的活跃状态Greeting的析构。  

#### **【A stop watch示例】**
　　Stop Watch有两个按钮，Start/Stop，Reset按钮。他还定义了三种状态，激活，运行和停止。其中激活状态包括运行和停止状态。  

　　我们来看下状态转换。  

　　激活状态：  
　　- 初始化转换为停止状态。  
　　- 按Reset按钮会转换为激活状态。  

　　运行状态：  
　　- 按Start/Stop按钮会转换为停止状态。  

　　停止状态：  
　　- 按Start/Stop按钮会转换为运行状态。  

　　代码示例如下：  
```c++
#include <boost/statechart/custom_reaction.hpp>
#include <boost/statechart/event.hpp>
#include <boost/statechart/simple_state.hpp>
#include <boost/statechart/state_machine.hpp>
#include <iostream>

struct EvStartStop : boost::statechart::event<EvStartStop> {};

struct EvReset : boost::statechart::event<EvReset> {};

struct Active;

struct StopWatch : boost::statechart::state_machine<StopWatch, Active> {};

struct Stopped;
struct Active : boost::statechart::simple_state<Active, StopWatch, Stopped> {
    typedef boost::statechart::custom_reaction<EvReset> reactions;

    Active() { std::cout << "Active" << std::endl; }
    boost::statechart::result react(const EvReset& event) {
        std::cout << "reset -> Active" << std::endl;
        return transit<Active>();
    }
};

struct Running : boost::statechart::simple_state<Running, Active> {
    typedef boost::statechart::custom_reaction<EvStartStop> reactions;

    Running() { std::cout << "Running" << std::endl; }
    boost::statechart::result react(const EvStartStop& event) {
        std::cout << "Running -> Stopped" << std::endl;
        return transit<Stopped>();
    }
};

struct Stopped : boost::statechart::simple_state<Stopped, Active> {
    typedef boost::statechart::custom_reaction<EvStartStop> reactions;

    Stopped() { std::cout << "Stopped" << std::endl; }
    boost::statechart::result react(const EvStartStop& event) {
        std::cout << "Stopped -> Running" << std::endl;
        return transit<Running>();
    }
};

int main() {
    StopWatch stop_watch;
    stop_watch.initiate();
    stop_watch.process_event(EvStartStop());
    stop_watch.process_event(EvStartStop());
    stop_watch.process_event(EvStartStop());
    stop_watch.process_event(EvReset());
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// Active
// Stopped
// Stopped -> Running
// Running
// Running -> Stopped
// Stopped
// Stopped -> Running
// Running
// reset -> Active
// Active
// Stopped
```