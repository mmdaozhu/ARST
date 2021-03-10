---
layout: post
title: ARST(十八)
date: 2020-08-17 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=blue>Algorithm</font>

#### **【LeetCode:112. Path Sum】**
　　Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.  

##### 　　Example:
```sh
Given the below binary tree and sum = 22,

      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/112.PathSum/PathSum.cpp  

###  <font color=blue>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
hierarchical | adj. 分层的；等级体系的
authoritative | adj. 有权威的；命令式的；当局的
stale | adj. 陈腐的；不新鲜的
propagation | n. 传播；繁殖；增殖

#### **【Comments and Conclusions】**

##### Domain name system

　　DNS is hierarchical, with a few authoritative servers at the top level. Your router or ISP provides information about which DNS server(s) to contact when doing a lookup. Lower level DNS servers cache mappings, which could become stale due to DNS propagation delays. DNS results can also be cached by your browser or OS for a certain period of time, determined by the time to live (TTL).  

　　Some DNS services can route traffic through various methods:  
　　- [Weighted round robin](https://g33kinfo.com/info/round-robin-vs-weighted-round-robin-lb/)  
　　　　- Prevent traffic from going to servers under maintenance  
　　　　- Balance between varying cluster sizes  
　　　　- A/B testing  
　　- [Latency-based](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-latency)  
　　- [Geolocation-based](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-geo)  

##### Disadvantage(s): DNS
　　- Accessing a DNS server introduces a slight delay, although mitigated by caching described above.  
　　- DNS server management could be complex and is generally managed by governments, ISPs, and large companies.  
　　- DNS services have recently come under DDoS attack.  
　
###  <font color=blue>tips: 阿里巴巴新任CTO鲁肃讲述：我是如何从一个写代码的程序员变成一个去思考技术战略的架构师？是支付宝成就了我。
</font>

https://www.bilibili.com/video/BV1UJ411t7au  

　　好的架构能解决的问题：  
　　　　- 为业务提供一个非常稳定的技术的基础设施，让业务能够持续无间断的运行（稳定）  
　　　　- 快速的实现业务（效率）  

　　对技术的要求越来越高，技术要成为业务创新的一个真正的赋能和真正的驱动力。  

###  <font color=blue>Share: Boost库下的策略模式</font>
　　策略模式： 定义一族算法类，将每个算法分别封装起来，让它们可以互相替换。策略模式可以使算法的变化独立于使用它们的客户端。  

　　Boost库中的大量函数对象就是策略模式的应用。  

　　Boost库中的大部分组件的模板参数也应用了策略模式，通过使用不同的模板类型，模板类会使用不同的算法。  

#### **【策略模式的应用】**
　　Boost库中的算法库下面有个模板函数minmax，提供了一个二元谓词模板参数。它可以改变minmax算法的行为。  
　　minmax的定义如下：  
```c++
namespace boost {

  template <typename T>
  tuple< T const&, T const& >
  minmax(T const& a, T const& b) {
    return (b<a) ? make_tuple(cref(b),cref(a)) : make_tuple(cref(a),cref(b));
  }

  template <typename T, class BinaryPredicate>
  tuple< T const&, T const& >
  minmax(T const& a, T const& b, BinaryPredicate comp) {
    return comp(b,a) ? make_tuple(cref(b),cref(a)) : make_tuple(cref(a),cref(b));
  }

} // namespace boost
```
　　接着我们拿minmax举例说明一下，代码如下：  
```c++
#include <boost/algorithm/minmax.hpp>
#include <boost/typeof/typeof.hpp>
#include <iostream>
#include <string>

struct Strategy1 {
    bool operator()(const std::string& a, const std::string& b) { return a.size() < b.size(); }
};

struct Strategy2 {
    bool operator()(const std::string& a, const std::string& b) {
        if (a.find("s2") != std::string::npos) {
            return true;
        }
        return a < b;
    }
};

int main() {
    BOOST_AUTO(x, boost::minmax(1, 2));
    std::cout << "min: " << x.get<0>() << ", max: " << x.get<1>() << std::endl;

    std::string s1 = "s12";
    std::string s2 = "s2";

    BOOST_AUTO(strategy_default, boost::minmax(s1, s2));
    std::cout << "min: " << strategy_default.get<0>() << ", max: " << strategy_default.get<1>() << std::endl;

    BOOST_AUTO(strategy1, boost::minmax(s1, s2, Strategy1()));
    std::cout << "min: " << strategy1.get<0>() << ", max: " << strategy1.get<1>() << std::endl;

    BOOST_AUTO(strategy2, boost::minmax(s1, s2, Strategy2()));
    std::cout << "min: " << strategy2.get<0>() << ", max: " << strategy2.get<1>() << std::endl;
    return 0;
}
//  g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// min: 1, max: 2
// min: s12, max: s2
// min: s2, max: s12
// min: s2, max: s12
```
　　我们使用了两个函数对象的策略，分别传给minmax算法，从而可以改变算法的行为。  
