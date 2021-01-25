---
layout: post
title: ARST(十)
date: 2020-06-22 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:70. Climbing Stairs】**

##### 　　Description:
　　You are climbing a stair case. It takes n steps to reach to the top.  

　　Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?  

##### 　　Example 1:
```sh
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

##### 　　Example 2:
```sh
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/070.ClimbingStairs/ClimbingStairs.cpp  

###  <font color=yellow>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  
http://www.lecloud.net/tagged/scalability/chrone
https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones
https://www.lecloud.net/post/7994751381/scalability-for-dummies-part-2-database

#### **【Words】**

English | Chinese
-|-
lengthy | adj. 漫长的，冗长的；啰唆的
evenly | adv. 均匀地；平衡地；平坦地；平等地
cluster | n. 群；簇；丛；串
reside | vi. 住，居住；属于
persistent | adj. 执着的，坚持不懈的；持续的，反复出现的
radical | adj. 激进的；根本的；彻底的
boldness | n. 大胆；冒失；显著
overtime | n. [劳经] 加班时间；延长时间；加时赛

#### **【Comments and Conclusions】**
　　The first golden rule for scalability: every server contains exactly the same codebase and does not store any user-related data, like sessions or profile pictures, on local disk or memory.  
　　Sessions need to be stored in a centralized data store which is accessible to all your application servers. It can be an external database or an external persistent cache.  
　　When database gets slower and slower, then denormalize right from the beginning and include no more Joins in any database query. You can switch to a better and easier to scale NoSQL database like MongoDB. Joins will now need to be done in your application code.  

###  <font color=yellow>tips：软件测试管理的四个阶段</font>
　　第一阶段：构建模块功能确认 BBFV(Building Block Functional Validation)  
　　第二阶段：系统设计验证 SDV(System Design Validation)  
　　- SDV Entry  
　　- SDV 100%  
　　- SDV Exit  
　　第三阶段：系统集成测试 SIT(System Integration Test)  
　　- SIT Entry  
　　- SIT 100%  
　　- SIT Exit  
　　第四阶段：系统验证测试 SVT(System Verification Test)  

###  <font color=yellow>Share: 实现一个multi_array库的Builder</font>
　　首先先了解一下建造者模式。  
　　如果一个类的创建非常复杂，设计到很多属性。那么我们先创建一个建造者，并且通过set()方法设置建造者的变量值，然后在使用build()方法真正创建对象之前，做集中的校验，校验通过之后才会创建对象。这个就是建造者模式。  

#### **【boost.multi_array库简介】**
　　multi_array是一个多维的容器，高效的实现了STL风格的多维数组。  
　　multi_array是递归定义的，它的每一个维度都是一个multi_array，最底层的则是个一维的multi_array。  

#### **【boost.multi_array库用法】**
```c++
#include <boost/array.hpp>
#include <boost/assign.hpp>
#include <boost/multi_array.hpp>
#include <iostream>

int main() {
    //通过extent_get类和预定义的实例extents来构造
    boost::multi_array<int, 3> ma(boost::extents[2][3][4]);
    int value(0);
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 4; k++) {
                ma[i][j][k] = value++;
            }
        }
    }

    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 4; k++) {
                std::cout << ma[i][j][k] << ",";
            }
            std::cout << std::endl;
        }
        std::cout << std::endl;
    }

    //通过array或者vector来构造，比extents快
    boost::multi_array<int, 3> mb(boost::array<std::size_t, 3>(boost::assign::list_of(2)(3)(4)));
    std::cout << mb.num_elements() << std::endl;
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 0,1,2,3,
// 4,5,6,7,
// 8,9,10,11,

// 12,13,14,15,
// 16,17,18,19,
// 20,21,22,23,

// 24
```

#### **【实现multi_array库的Builder】**
　　使用生成器模式可以把创建对象的过程封装起来。可以在multi_array的两种创建方式间随意切换，而不会影响到使用multi_array的客户代码。  
```c++
#include <boost/array.hpp>
#include <boost/multi_array.hpp>
#include <boost/noncopyable.hpp>
#include <boost/shared_ptr.hpp>
#include <boost/static_assert.hpp>
#include <boost/typeof/typeof.hpp>
#include <iostream>

template <typename T, std::size_t NumDims>
class multi_builder : boost::noncopyable {
public:
    typedef boost::multi_array<T, NumDims> array_type;
    typedef boost::shared_ptr<array_type> type;

public:
    multi_builder() {}
    ~multi_builder() {}

    template <std::size_t n>
    void dim(std::size_t x) {
        BOOST_STATIC_ASSERT(n >= 0 && n < NumDims);
        d[n] = x;
    }
    type create() { return type(new array_type(d)); }

    type create1() {
        typedef boost::detail::multi_array::extent_gen<NumDims> gen_type;
        gen_type r;
        for (int i = 0; i != NumDims; ++i) {
            typedef typename gen_type::range range_type;
            r.ranges_[i] = range_type(0, d[i]);
        }
        return type(new array_type(r));
    }

private:
    boost::array<std::size_t, NumDims> d;
};

int main() {
    multi_builder<int, 3> builder;
    builder.dim<0>(2);
    builder.dim<1>(3);
    builder.dim<2>(4);
    BOOST_AUTO(mp, builder.create());
    int value(0);
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 4; k++) {
                (*mp)[i][j][k] = value++;
            }
        }
    }

    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 4; k++) {
                std::cout << (*mp)[i][j][k] << ",";
            }
            std::cout << std::endl;
        }
        std::cout << std::endl;
    }
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 0,1,2,3,
// 4,5,6,7,
// 8,9,10,11,

// 12,13,14,15,
// 16,17,18,19,
// 20,21,22,23,
```