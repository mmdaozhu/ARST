---
layout: post
title: ARST(十四)
date: 2020-07-20 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=green>Algorithm</font>

#### **【LeetCode:8. String to Integer (atoi)】**

##### 　　Description:
　　Implement atoi which converts a string to an integer.  
　　The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.  
　　The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.  
　　If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.  
　　If no valid conversion could be performed, a zero value is returned.  

##### 　　Example 1:
```sh
Input: str = "42"
Output: 42
```

##### 　　Example 2:
```sh
Input: str = "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign. Then take as many numerical digits as possible, which gets 42.
```

##### 　　Example 3:
```sh
Input: str = "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

##### 　　Example 4:
```sh
Input: str = "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical digit or a +/- sign. Therefore no valid conversion could be performed.
```

##### 　　Example 5:
```sh
Input: str = "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer. Thefore INT_MIN (−231) is returned.
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/008.StringToInteger(atoi)/StringToInteger(atoi).cpp

###  <font color=green>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  
https://robertgreiner.com/cap-theorem-revisited/  

#### **【Words】**
English | Chinese
-|-
commodity | n. 商品，货物；日用品
penalty | n. 罚款，罚金；处罚
incur | v. 招致，遭受；引致，带来

#### **【Comments and Conclusions】**
　　The CAP Theorem states that, in a distributed system, you can only have two out of the following three guarantees across a write/read pair: Consistency, Availability and Partition Tolerance.  
　　- Consistency: A read is guaranteed to return the most recent write for a given client.  
　　- Availability: A non-failing node will return a reasonable response within a reasonable amount of time.(no error or no timeout)  
　　- Partition Tolerance: The system will continue to function when network partitions occur.  
　　Networks aren't reliable, so you'll need to support partition tolerance. You'll need to make a software tradeoff between consistency and availability.  
　　- CP: Wait for a response from the partitioned node which could result in a timeout error. The system can also choose to return an error, depending on the scenario you desire. Choose Consistency over Availability when your business requirements dictate atomic reads and writes.  
　　- AP: availability and partition tolerance Responses return the most readily available version of the data available on any node, which might not be the latest. Writes might take some time to propagate when the partition is resolved.  
　　The decision between Consistency and Availability is a software trade off.  

###  <font color=green>tips: TDengine 的超强性能是如何实现的？</font>
https://www.bilibili.com/video/BV1dE411e7Vf

#### **【物联网、工业4.0数据特征：时序空间数据】**
　　1.所有采集的数据都是时序的  
　　2.数据都是结构化的  
　　3.一个采集点的数据源是唯一的  
　　4.数据很少有更新或删除操作  
　　5.数据一般是按到期日期来删除的  
　　6.数据以写操作为主，读操作为辅  
　　7.数据流量平稳、可以较为准确的计算  
　　8.数据都有统计、聚合等实时计算操作  
　　9.数据一定是指定时间段和指定区域查找的  
　　10.数据量巨大，一天的数据量就超过100亿条  

#### **【TDengine 应运而生】**
　　https://github.com/taosdata/TDengine  

#### **【TDengine 提供的主要功能】**
　　1.实时数据库，历史数据库，操作合一透明  
　　2.流式计算，对一个或多个数据流实时聚合计算  
　　3.缓存，所有设备最新记录实时返回  
　　4.数据订阅，最新的数据可实时推送到应用  

#### **【TDengine 的产品竞争力】**
　　1.10倍以上的性能提升  
　　2.成本大幅下降  
　　3.零学习成本  

#### **【TDengine 应用领域】**
　　物联网  

#### **【TDengine 的技术介绍】**
　　架构借鉴cassandra，HBase，Redis  
　　数据模型：一台设备一张表  
　　采取列式存储，便于压缩；不通数据采用不通压缩算法  
　　超级表：多个采集点的数据聚合  
　　实时Stream计算  
　　零管理：最大程度的减少运维工作  

###  <font color=green>Share: boost.operators库下的基类链</font>

#### **【operators库的用法】**
　　operators库允许用户在自己的类里仅定义少量的操作符，就可以方便地自动生成其他操作符重载。  
```c++
#include <boost/operators.hpp>
#include <cassert>
#include <iostream>

class Point : boost::less_than_comparable<Point>, boost::equality_comparable<Point> {
public:
    explicit Point(int a = 0, int b = 0, int c = 0) : x(a), y(b), z(c) {}

    friend bool operator<(const Point& l, const Point& r) {
        return (l.x * l.x + l.y * l.y + l.z * l.z) < (r.x * r.x + r.y * r.y + r.z * r.z);
    }

    friend bool operator==(const Point& l, const Point& r) { return l.x == r.x && l.y == r.y && l.z == r.z; }

    void print() const {
        std::cout << "x = " << x << std::endl;
        std::cout << "y = " << y << std::endl;
        std::cout << "z = " << z << std::endl;
    }

private:
    int x;
    int y;
    int z;
};

int main() {
    Point p0;
    Point p1(1, 2, 3);
    Point p2(3, 0, 5);
    Point p3(3, 2, 1);
    Point p4(p1);
    assert(p0 < p1 && p1 < p2);
    assert(p2 > p0);
    assert(p1 <= p3);
    assert(!(p1 < p3) && !(p1 > p3));
    assert(p1 == p4);
    assert(p1 != p2);
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
```

#### **【operators库的源码剖析】**
　　直接上less_than_comparable源码:  
```c++
# define BOOST_OPERATOR_TEMPLATE(template_name)                                       \
template <class T                                                                     \
         ,class U = T                                                                 \
         ,class B = operators_detail::empty_base<T>                                   \
         ,class O = typename is_chained_base<U>::value                                \
         >                                                                            \
struct template_name;                                                                 \
                                                                                      \
template<class T, class U, class B>                                                   \
struct template_name<T, U, B, operators_detail::false_t>                              \
  : template_name##2<T, U, B> {};                                                     \
                                                                                      \
template<class T, class U>                                                            \
struct template_name<T, U, operators_detail::empty_base<T>, operators_detail::true_t> \
  : template_name##1<T, U> {};                                                        \
                                                                                      \
template <class T, class B>                                                           \
struct template_name<T, T, B, operators_detail::false_t>                              \
  : template_name##1<T, B> {};                                                        \
                                                                                      \
template<class T, class U, class B, class O>                                          \
struct is_chained_base< template_name<T, U, B, O> > {                                 \
  typedef operators_detail::true_t value;                                             \
};                                                                                    \
                                                                                      \

BOOST_OPERATOR_TEMPLATE(less_than_comparable)
```
　　展开BOOST_OPERATOR_TEMPLATE(less_than_comparable)之后，代码如下：  
```c++
struct true_t {};
struct false_t {};

template <typename T> class empty_base {};

template <class T, class U, class B = operators_detail::empty_base<T> >
struct less_than_comparable2 : B
{
     friend bool operator<=(const T& x, const U& y) { return !static_cast<bool>(x > y); }
     friend bool operator>=(const T& x, const U& y) { return !static_cast<bool>(x < y); }
     friend bool operator>(const U& x, const T& y)  { return y < x; }
     friend bool operator<(const U& x, const T& y)  { return y > x; }
     friend bool operator<=(const U& x, const T& y) { return !static_cast<bool>(y < x); }
     friend bool operator>=(const U& x, const T& y) { return !static_cast<bool>(y > x); }
};

template <class T, class B = operators_detail::empty_base<T> >
struct less_than_comparable1 : B
{
     friend bool operator>(const T& x, const T& y)  { return y < x; }
     friend bool operator<=(const T& x, const T& y) { return !static_cast<bool>(y < x); }
     friend bool operator>=(const T& x, const T& y) { return !static_cast<bool>(x < y); }
};

template <class T
         ,class U = T
         ,class B = operators_detail::empty_base<T>
         ,class O = typename is_chained_base<U>::value
         >
struct less_than_comparable;

template<class T, class U, class B>
struct less_than_comparable<T, U, B, operators_detail::false_t>
  : less_than_comparable2<T, U, B> {};

template<class T, class U>
struct less_than_comparable<T, U, operators_detail::empty_base<T>, operators_detail::true_t>
  : less_than_comparable1<T, U> {};

template <class T, class B>
struct less_than_comparable<T, T, B, operators_detail::false_t>
  : less_than_comparable1<T, B> {};

template<class T, class U, class B, class O>
struct is_chained_base< less_than_comparable<T, U, B, O> > {
  typedef operators_detail::true_t value;
};
```
　　less_than_comparable使用基类链技术解决了多重继承的问题。我们举个例子就明白了：  
```c++
less_than_comparable<Point, boost::equality_comparable<Point> > 

//展开之后
less_than_comparable1<Point, boost::equality_comparable<Point> >
struct less_than_comparable1<Point> : boost::equality_comparable<Point>
```

#### **【operators库的基类链用法】**
```cpp
class Point : 
    boost::less_than_comparable<Point, 
    boost::equality_comparable<Point,
    boost::addable<Point,
    > > >
{};
```
