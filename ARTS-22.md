---
layout: post
title: ARST(二十二)
date: 2020-09-14 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=indigo>Algorithm</font>

#### **【LeetCode:120. Triangle】**

##### 　　Description:
　　Given a triangle array, return the minimum path sum from top to bottom.  

　　For each step, you may move to an adjacent number of the row below. More formally, if you are on index i on the current row, you may move to either index i or index i + 1 on the next row.  

##### 　　Example 1:
```sh
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).
```

##### 　　Example 2:
```sh
Input: triangle = [[-10]]
Output: -10
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/120.Triangle/Triangle.cpp  

###  <font color=indigo>Review</font>

https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
advocate | v. 提倡，拥护；为……辩护
monolithic | adj. 整体的；巨石的，庞大的；完全统一的

#### **【Comments and Conclusions】**

##### Application layer
　　Separating out the web layer from the application layer (also known as platform layer) allows you to scale and configure both layers independently. Adding a new API results in adding application servers without necessarily adding additional web servers.  

##### Microservices
　　Related to this discussion are microservices, which can be described as a suite of independently deployable, small, modular services. Each service runs a unique process and communicates through a well-defined, lightweight mechanism to serve a business goal.  

##### Service Discovery
　　Systems such as Consul, Etcd, and Zookeeper can help services find each other by keeping track of registered names, addresses, and ports. Health checks help verify service integrity and are often done using an HTTP endpoint. Both Consul and Etcd have a built in key-value store that can be useful for storing config values and other shared data.  

##### Disadvantage(s): application layer
　　- Adding an application layer with loosely coupled services requires a different approach from an architectural, operations, and process viewpoint (vs a monolithic system).  
　　- Microservices can add complexity in terms of deployments and operations.  
　
###  <font color=indigo>tips: 京东首席架构师带你回顾过去十年架构领域最重要的三个变化</font>

https://www.bilibili.com/video/BV1dJ411G7Yo  

　　三个重要的变化  

　　- 容器：极大简化了运维的复杂性。  
　　- 多元数据库：解决数据存储、管理和分析的问题。  
　　- 架构：系统跟算法的结合越来越紧密。  

###  <font color=indigo>Share: boost.variant库下的访问者模式</font>
　　访问者模式允许一个或者多个操作应用到一组对象上，设计意图是解耦操作和对象本身，保持类职责单一、满足开闭原则以及应对代码的复杂性。  

#### **【boost.variant库简介】**
　　variant是对c/c++中union概念的增强和扩展。普通的union只能持有POD，而不能持有如string、vector等复杂类型，variant没有这个限制。  

　　variant类定义如下：  
```c++
template <
      typename T0_
    , BOOST_VARIANT_ENUM_SHIFTED_PARAMS(typename T)
    >
class variant
{
public: // structors

    ~variant() BOOST_NOEXCEPT
    {
        destroy_content();
    }

    variant()
#if !(defined(__SUNPRO_CC) && BOOST_WORKAROUND(__SUNPRO_CC, <= 0x5130))
              BOOST_NOEXCEPT_IF(boost::has_nothrow_constructor<internal_T0>::value)
#endif
    {
#ifdef _MSC_VER
#pragma warning( push )
// behavior change: an object of POD type constructed with an initializer of the form () will be default-initialized
#pragma warning( disable : 4345 )
#endif
        // NOTE TO USER :
        // Compile error from here indicates that the first bound
        // type is not default-constructible, and so variant cannot
        // support its own default-construction.
        //
        new( storage_.address() ) internal_T0();
        indicate_which(0); // zero is the index of the first bounded type
#ifdef _MSC_VER
#pragma warning( pop )
#endif
    }


public: // structors, cont.

    template <typename T>
    variant(const T& operand,
        typename boost::enable_if<mpl::or_<
            mpl::and_<
                mpl::not_< boost::is_same<T, variant> >,
                boost::detail::variant::is_variant_constructible_from<const T&, internal_types>
            >,
            boost::is_same<T, boost::recursive_variant_> >,
            bool >::type = true)
    {
        convert_construct(operand, 1L);
    }

    template <typename T>
    variant(
          T& operand
        , typename boost::enable_if<mpl::or_<
            mpl::and_<
                mpl::not_< is_const<T> >,
                mpl::not_< boost::is_same<T, variant> >,
                boost::detail::variant::is_variant_constructible_from<T&, internal_types>
            >,
            boost::is_same<T, boost::recursive_variant_> >,
            bool >::type = true
        )
    {
        convert_construct(operand, 1L);
    }


public: // queries

    int which() const BOOST_NOEXCEPT
    {
        // If using heap backup...
        if (using_backup())
            // ...then return adjusted which_:
            return -(which_ + 1);

        // Otherwise, return which_ directly:
        return which_;
    }

    bool empty() const BOOST_NOEXCEPT
    {
        return false;
    }

    const boost::typeindex::type_info& type() const
    {
        detail::variant::reflect visitor;
        return this->apply_visitor(visitor);
    }
};
```

　　带参数的构造函数参数可以是模板类型中的任意类型，varaint将初始化为这种类型。  

　　which()函数可以返回当前值的类型在模板参数列表的索引号。type()函数用来检测当前variant持有的对象。  

#### **【访问元素】**
　　variant通过使用get()函数来访问元素，但是get()存在类型不安全的隐患，因此要使用异常捕获。  

　　代码如下：  
```c++
#include <boost/variant.hpp>
#include <iostream>
#include <string>

int main() {
    using vart_t = boost::variant<int, double, std::string>;
    vart_t v;
    std::cout << v.type().name() << std::endl;
    std::cout << v.which() << std::endl;

    v = "variant demo";
    std::cout << *boost::get<std::string>(&v) << std::endl;

    try {
        std::cout << boost::get<double>(v) << std::endl;
    } catch (boost::bad_get&) {
        std::cout << "bad_get" << std::endl;
    }
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// i
// 0
// variant demo
// bad_get
```

　　第二种方式可以在运行时通过type()来检测variant的类型，再施以必要的操作，比如：  
```c++
#include <boost/variant.hpp>
#include <iostream>
#include <string>
#include <typeinfo>
using vart_t = boost::variant<int, double, std::string>;

void var_print(vart_t &v) {
    if (v.type() == typeid(int)) {
        std::cout << boost::get<int>(v) << std::endl;
    } else if (v.type() == typeid(double)) {
        std::cout << boost::get<double>(v) << std::endl;
    } else if (v.type() == typeid(std::string)) {
        std::cout << boost::get<std::string>(v) << std::endl;
    } else {
        std::cout << "dont't know type" << std::endl;
    }
}

int main() {
    vart_t v;
    var_print(v);

    v = 1.5;
    var_print(v);

    v = "variant demo";
    var_print(v);
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 0
// 1.5
// variant demo
```
　　使用RTTI技术，效率低，并且一旦variant的模板参数发生变化，var_print()函数也必须改动，很不优雅。  

#### **【访问者模式】**
　　variant基于访问者模式提供了模板类static_visitor，它解耦了variant的数据存储和访问操作，把访问操作集中在访问器类，易于增加新的访问操作，使这两者可以彼此独立的变化。  

　　static_visitor是一个静态访问variant的基类，它应该被实际访问器函数对象所继承来使用，定义如下：  
```c++
template <typename R = ::boost::detail::static_visitor_default_return>
class static_visitor
    : public detail::is_static_visitor_tag
{
public: // typedefs

    typedef R result_type;

protected: // for use as base class only
#if !defined(BOOST_NO_CXX11_DEFAULTED_FUNCTIONS) && !defined(BOOST_NO_CXX11_NON_PUBLIC_DEFAULTED_FUNCTIONS)
    static_visitor() = default;
#else
    static_visitor()  BOOST_NOEXCEPT { }
#endif
};
```
　　模板参数R指明了访问者子类operator()的返回值类型，默认是void。  

　　下面我们看个例子：  
```c++
#include <boost/variant.hpp>
#include <iostream>
#include <string>
#include <typeinfo>
#include <vector>

using vart_t = boost::variant<int, double, std::string, std::vector<int>>;

struct var_print : public boost::static_visitor<> {
    template <typename T>
    void operator()(T& i) const {
        std::cout << i << std::endl;
    }
};

template <>
void var_print::operator()<std::vector<int>>(std::vector<int>& v) const {
    for (const auto& value : v) {
        std::cout << value << std::endl;
    }
}

int main() {
    var_print vp;

    vart_t v;
    boost::apply_visitor(vp, v);

    v = 1.5;
    boost::apply_visitor(vp, v);

    v = "variant demo";
    boost::apply_visitor(vp, v);

    std::vector<int> vi{1, 2, 3, 4, 5};
    v = vi;
    boost::apply_visitor(vp, v);
    return 0;
}

// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 0
// 1.5
// variant demo
// 1
// 2
// 3
// 4
// 5
```

　　子类继承了var_print并实现了operator()函数，用来访问variant的内部值。  

　　apply_visitor()函数中variant对象会执行var_print中定义的操作。  

　　看一下相关源码。  
```c++
template <typename Visitor, typename Visitable>
inline typename Visitor::result_type
apply_visitor(Visitor& visitor, Visitable& visitable)
{
    return visitable.apply_visitor(visitor);
}

template <
      typename T0_
    , BOOST_VARIANT_ENUM_SHIFTED_PARAMS(typename T)
    >
class variant
{
public: 
    template <typename Visitor>
    typename Visitor::result_type
    apply_visitor(Visitor& visitor)
#ifndef BOOST_NO_CXX11_REF_QUALIFIERS
    &
#endif
    {
        detail::variant::invoke_visitor<Visitor, false> invoker(visitor);
        return this->internal_apply_visitor(invoker);
    }

    template <typename Visitor>
    BOOST_FORCEINLINE typename Visitor::result_type
    internal_apply_visitor(Visitor& visitor)
    {
        return internal_apply_visitor_impl(
              which_, which(), visitor, storage_.address()
            );
    }
};
```

　　在variant类中最终会把Visitor传递给internal_apply_visitor_impl()函数，最终回调var_print类中的operator()()函数。  

　　当然，如果你对variant类对象中的数据有其他不同的操作，再定义一个类继承static_visitor类即可。  