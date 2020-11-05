---
layout: post
title: ARST(九)
date: 2020-06-15 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:239.Sliding Window Maximum】**

##### 　　Description:
　　Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right.  
　　You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.  

##### 　　Follow up:
　　Could you solve it in linear time?  

##### 　　Example:
```sh
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

##### 　　Constraints:
```sh
1 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4
1 <= k <= nums.length
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/239.SlidingWindowMaximum/SlidingWindowMaximum.cpp

###  <font color=yellow>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  
https://www.youtube.com/watch?v=-W9F__D3oY4  

#### **【Comments and Conclusions】**

##### 1. Replication
　　- Master-Slave  
　　- Master-Master  

##### 2. Partitioning

##### 3. Data Center Redundancy

##### 4. Security
　　- SSL  
　
###  <font color=yellow>tips：Postman的Collection功能</font>
　　Postman可以通过创建Collection来保存一系列相关Request请求。方便查看、导入导出。  
![hello](/ARTS-9/1.png)

###  <font color=yellow>Share：简析Boost库中的Functional/Factory库</font>
　　首先先了解一下工厂模式。  
　　工厂模式用来创建不同但是相关类型的对象（继承同一父类或者接口的一组子类），由给定的参数来决定创建哪种类型的对象。实际上，如果创建对象的逻辑并不复杂，那我们直接通过new来创建对象就可以了，不需要使用工厂模式。当创建逻辑比较复杂，是一个“大工程”的时候，我们就考虑使用工厂模式，封装对象的创建过程，将对象的创建和使用相分离。  

#### **【boost.Functional中Factory库简介】**
　　Factory模板库主要是将new表达式封装成一个函数对象，后面通过回调机制来构造不同的对象，回调时可以传递一些参数用于构造。  
　　相比于传统的c++工厂模式的实现，它的优点主要体现在：不需要定义一个抽象的factory基类和一系列继承factory的子类。  

#### **【boost.Functional中Factory库定义】**
　　factory的定义如下：  
```c++
namespace boost {

enum factory_alloc_propagation {
    factory_alloc_for_pointee_and_deleter,
    factory_passes_alloc_to_smart_pointer
};

template<class Pointer, class Allocator = void,
    factory_alloc_propagation Policy = factory_alloc_for_pointee_and_deleter>
class factory;

template<class Pointer, factory_alloc_propagation Policy>
class factory<Pointer, void, Policy> {
public:
    typedef typename remove_cv<Pointer>::type result_type;

private:
    typedef typename pointer_traits<result_type>::element_type type;

public:
#if !defined(BOOST_NO_CXX11_RVALUE_REFERENCES) && \
    !defined(BOOST_NO_CXX11_VARIADIC_TEMPLATES)
    template<class... Args>
    result_type operator()(Args&&... args) const {
        return result_type(new type(std::forward<Args>(args)...));
    }
#else
    result_type operator()() const {
        return result_type(new type());
    }

    template<class A0>
    result_type operator()(A0& a0) const {
        return result_type(new type(a0));
    }

    template<class A0, class A1>
    result_type operator()(A0& a0, A1& a1) const {
        return result_type(new type(a0, a1));
    }

    template<class A0, class A1, class A2>
    result_type operator()(A0& a0, A1& a1, A2& a2) const {
        return result_type(new type(a0, a1, a2));
    }

    template<class A0, class A1, class A2, class A3>
    result_type operator()(A0& a0, A1& a1, A2& a2, A3& a3) const {
        return result_type(new type(a0, a1, a2, a3));
    }

    template<class A0, class A1, class A2, class A3, class A4>
    result_type operator()(A0& a0, A1& a1, A2& a2, A3& a3, A4& a4) const {
        return result_type(new type(a0, a1, a2, a3, a4));
    }

    template<class A0, class A1, class A2, class A3, class A4, class A5>
    result_type operator()(A0& a0, A1& a1, A2& a2, A3& a3, A4& a4,
        A5& a5) const {
        return result_type(new type(a0, a1, a2, a3, a4, a5));
    }

    template<class A0, class A1, class A2, class A3, class A4, class A5,
        class A6>
    result_type operator()(A0& a0, A1& a1, A2& a2, A3& a3, A4& a4, A5& a5,
        A6& a6) const {
        return result_type(new type(a0, a1, a2, a3, a4, a5, a6));
    }

    template<class A0, class A1, class A2, class A3, class A4, class A5,
        class A6, class A7>
    result_type operator()(A0& a0, A1& a1, A2& a2, A3& a3, A4& a4, A5& a5,
        A6& a6, A7& a7) const {
        return result_type(new type(a0, a1, a2, a3, a4, a5, a6, a7));
    }

    template<class A0, class A1, class A2, class A3, class A4, class A5,
        class A6, class A7, class A8>
    result_type operator()(A0& a0, A1& a1, A2& a2, A3& a3, A4& a4, A5& a5,
        A6& a6, A7& a7, A8& a8) const {
        return result_type(new type(a0, a1, a2, a3, a4, a5, a6, a7, a8));
    }

    template<class A0, class A1, class A2, class A3, class A4, class A5,
        class A6, class A7, class A8, class A9>
    result_type operator()(A0& a0, A1& a1, A2& a2, A3& a3, A4& a4, A5& a5,
        A6& a6, A7& a7, A8& a8, A9& a9) const {
        return result_type(new type(a0, a1, a2, a3, a4, a5, a6, a7, a8, a9));
    }
#endif
};
```
　　分析一下源码：  
```c++
    typedef typename remove_cv<Pointer>::type result_type;
    typedef typename pointer_traits<result_type>::element_type type;
```
　　remove_cv的根据名字可以看出，是去掉const, volatile修饰符。  
　　pointer_traits其实就是取Pointer指向的数据类型。  
　　其他的成员函数没啥好说的，就是将new表达式封装成一个函数对象。  

#### **【boost.Functional中Factory库用法】**
```c++
#include <boost/function.hpp>
#include <boost/functional/factory.hpp>
#include <iostream>
#include <map>
#include <string>

class BaseAPP {
public:
    virtual ~BaseAPP() {}
    virtual void Running() = 0;
};

class A_APP : public BaseAPP {
public:
    A_APP() { std::cout << "A_APP constructor" << std::endl; }
    ~A_APP() { std::cout << "A_APP destructor" << std::endl; }
    virtual void Running() { std::cout << "A Running" << std::endl; }
};

class B_APP : public BaseAPP {
public:
    B_APP() { std::cout << "B_APP constructor" << std::endl; }
    ~B_APP() { std::cout << "B_APP destructor" << std::endl; }
    virtual void Running() { std::cout << "B Running" << std::endl; }
};

class APPFactoryMap {
    typedef boost::function<BaseAPP*()> BaseAPPFunction;

public:
    APPFactoryMap() {
        factories["A"] = boost::factory<A_APP*>();
        factories["B"] = boost::factory<B_APP*>();
    }
    BaseAPP* createAPP(const std::string name) {
        std::map<std::string, BaseAPPFunction>::iterator it = factories.begin();
        for (; it != factories.end(); it++) {
            if (it->first == name) {
                return it->second();
            }
        }
        return nullptr;
    }

private:
    std::map<std::string, BaseAPPFunction> factories;
};

int main() {
    APPFactoryMap map;
    BaseAPP* app = map.createAPP("A");
    if (app != nullptr) {
        app->Running();
    }
    return 0;
}

// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// A_APP constructor
// A Running
```
