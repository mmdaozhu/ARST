---
layout: post
title: ARST(十一)
date: 2020-06-29 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:69. Sqrt(x)】**

##### 　　Description:
　　Implement int sqrt(int x).  
　　Compute and return the square root of x, where x is guaranteed to be a non-negative integer.  
　　Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.  

##### 　　Example 1:
```sh
Input: 4
Output: 2
```

##### 　　Example 2:
```sh
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/069.Sqrt(x)/Sqrt(x).cpp

###  <font color=yellow>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  
http://www.lecloud.net/tagged/scalability/chrone  
https://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache  
https://www.lecloud.net/post/9699762917/scalability-for-dummies-part-4-asynchronism  

#### **【Words】**

English | Chinese
-|-
asynchronous | adj. [电] 异步的；不同时的；不同期的
synchronous | adj. 同步的；同时的

#### **【Comments and Conclusions】**
　　There are 2 patterns of caching your data.  
　　The old one is to cache database queries. It's not recommended whening cacheing a complex query.  
　　The new one is to cache objects. Let your class assemble a dataset from your database and then store the complete instance of the class or the assembled dataset in the cache. It sounds theoretical.It makes asynchronous processing possible.  
　　There are 2 paradigms asynchronism.  
　　The first one is to do the time-consuming work in advance and serving the finished work with a low request time.  
　　The second one is to handle tasks asynchronously. The frontend of your website sends a job is in work, please continue to browse the page. The job queue is constantly checked by a bunch of workers for new jobs. If there is a new job then the worker does the job and after some minutes sends a signal that the job was done.  

###  <font color=yellow>tips：左耳朵耗子的经验分享</font>
https://www.bilibili.com/video/BV1ct411i7uc
　　在路透公司的线上bug，fix bug的code没发布出去。
　　撑着年轻，多犯错误，对问题才会有深刻的理解。
　　不断学习不断总结。不要畏手畏脚，大胆做。
https://www.bilibili.com/video/BV1Ft411i7qq
　　欣赏37Signals公司，小团队大产出。
　　条件受限，才会关注自己做的东西。
　　条件受限，才会逼着自己和别人合作。
　　条件受限，才会逼着自己自动化。

　　所有的创新都与自动化有关。

https://www.bilibili.com/video/BV1Ft411i7i8
　　辞职（所有人都反对）。

###  <font color=yellow>Share: enable_shared_from_this 下的原型模式</font>
　　首先了解一下原型模式。  
　　如果对象的创建成本比较大，而同一个类的不同对象之间差别不大（大部分字段都相同），在这种情况下，我们可以利用对已有对象（原型）进行复制（或者叫拷贝）的方式来创建新对象，以达到节省创建时间的目的。这种基于原型来创建对象的方式就叫作原型设计模式（Prototype Design Pattern），简称原型模式。  

#### **【enable_shared_from_this库简介】**
　　类模板enable_shared_from_this 用作父类，通过它的成员函数获取指向自身的shared_ptr 或者 weak_ptr 。  

#### **【enable_shared_from_this库用法】**
```c++
#include <boost/enable_shared_from_this.hpp>
#include <boost/shared_ptr.hpp>
#include <cassert>
#include <iostream>

class Y : public boost::enable_shared_from_this<Y> {
public:
    Y(int n) : x(n) {}
    void print() { std::cout << "Y.x: " << x << std::endl; }
    boost::shared_ptr<Y> f() { return shared_from_this(); }
    int x;
};

int main() {
    boost::shared_ptr<Y> p(new Y(10));
    p->print();
    boost::shared_ptr<Y> q = p->f();
    assert(p == q);
    q->x = 1000;
    q->print();
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// Y.x: 10
// Y.x: 1000
```
　　类Y继承了enable_shared_from_this类，通过成员函数f()返回this的shared_ptr。  

#### **【enable_shared_from_this库源码剖析】**
```c++
namespace boost
{
public:

    shared_ptr<T> shared_from_this()
    {
        shared_ptr<T> p( weak_this_ );
        BOOST_ASSERT( p.get() == this );
        return p;
    }

    shared_ptr<T const> shared_from_this() const
    {
        shared_ptr<T const> p( weak_this_ );
        BOOST_ASSERT( p.get() == this );
        return p;
    }

    weak_ptr<T> weak_from_this() BOOST_SP_NOEXCEPT
    {
        return weak_this_;
    }

    weak_ptr<T const> weak_from_this() const BOOST_SP_NOEXCEPT
    {
        return weak_this_;
    }

public: // actually private, but avoids compiler template friendship issues

    // Note: invoked automatically by shared_ptr; do not call
    template<class X, class Y> void _internal_accept_owner( shared_ptr<X> const * ppx, Y * py ) const BOOST_SP_NOEXCEPT
    {
        if( weak_this_.expired() )
        {
            weak_this_ = shared_ptr<T>( *ppx, py );
        }
    }

private:

    mutable weak_ptr<T> weak_this_;
};
```
　　我们来看shared_from_this()成员函数，其中执行这行代码时shared_ptr<T> p( weak_this_ )时会调用_internal_accept_owner成员函数。  
　　现在我来看下shared_ptr里面的实现：  
```c++
    template<class Y>
    explicit shared_ptr( Y * p ): px( p ), pn() // Y must be complete
    {
        boost::detail::sp_pointer_construct( this, p, pn );
    }

    template< class T, class Y > inline void sp_pointer_construct( boost::shared_ptr< T > * ppx, Y * p, boost::detail::shared_count & pn )
    {
        boost::detail::shared_count( p ).swap( pn );
        boost::detail::sp_enable_shared_from_this( ppx, p, p );
    }

    template< class X, class Y, class T > inline void sp_enable_shared_from_this( boost::shared_ptr<X> const * ppx, Y const * py,     boost::enable_shared_from_this< T > const * pe )
    {
        if( pe != 0 )
        {
            pe->_internal_accept_owner( ppx, const_cast< Y* >( py ) );
        }
    }
```
　　经过一系列的调用，通过weak_ptr，shared_ptr<T>持有this指针。  

