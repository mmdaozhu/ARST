---
layout: post
title: ARST(四十三)
date: 2021-02-08 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=green>Algorithm</font>

#### **【LeetCode:300. Longest Increasing Subsequence】**

##### 　　Description:
　　Given an integer array nums, return the length of the longest strictly increasing subsequence.  

　　A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].  

##### 　　Example 1:
```sh
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

##### 　　Example 2:
```sh
Input: nums = [0,1,0,3,2,3]
Output: 4
```

##### 　　Example 3:
```sh
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

##### 　　C++ Solution 1:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/300.LongestIncreasingSubsequence/LongestIncreasingSubsequence1.cpp  

##### 　　C++ Solution 2:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/300.LongestIncreasingSubsequence/LongestIncreasingSubsequence2.cpp  

###  <font color=green>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/c-20-concepts-what-we-dont-get  
　　C++20: Concepts - What we don't get  

#### **【Words】**
English | Chinese
-|-
riddle | n. 谜语
obscure | adj. 昏暗的，朦胧的
sceptical | adj. 怀疑的
interchangeably | adv. [数] 可交换地

#### **【Comments and Conclusions】**

##### Template Introduction
　　Let me first start with a short riddle. Which one, of the following syntactic variants, is not possible with the concepts draft?  
```c++
template <typename T>  // (1)
requires Integral<T> T gcd(T a, T b) {
    if (b == 0)
        return a;
    else
        return gcd(b, a % b);
}

template <typename T>  // (2)
T gcd1(T a, T b) requires Integral<T> {
    if (b == 0) {
        return a;
    } else
        return gcd(b, a % b);
}

template <Integral T>  // (3)
T gcd2(T a, T b) {
    if (b == 0) {
        return a;
    } else
        return gcd(b, a % b);
}

Integral auto gcd3(Integral auto a, Integral auto b) {  // (4)
    if (b == 0) {
        return a;
    } else
        return gcd(b, a % b);
}

Integral{T}  // (5)
Integral
gcd(T a, T b) {
    if (b == 0) {
        return a;
    } else {
        return gcd(b, a % b);
    }
}
```
　　Maybe, you don't know. So let me name the five variants.  

　　1. Requires clause  
　　2. Trailing requires clause  
　　3. Constrained template parameters  
　　4. Abbreviated Function Templates  
　　5. Template Introduction  

　　I assume you have chosen the most obscure one: Template Introduction. You are right. Maybe we get it with a later C++ standard, but I'm highly sceptical. In my talks in the last year, I heard no one complaining that they are not part of C++20. I'm also not a big fan of Template Introduction because they introduced a new asymmetry, and you know, I'm not a fan of asymmetries.  

##### The Asymmetry
　　Instead of declaring your constrained template by using template<Integral T>, you can just Integral{T}. Here, you see the asymmetry. You can only use Template Introduction with concepts (constrained placeholders) but not with auto (unconstrained placeholders). The following example makes my point. This and the following example is based on the previous concepts TS specification and compiled with the GCC.  

```c++
#include <iostream>
#include <type_traits>

template <typename T>
concept bool Integral() {
    return std::is_integral<T>::value;
}

Integral{T}  // (1)
T gcd(T a, T b) {
    if (b == 0) {
        return a;
    } else {
        return gcd(b, a % b);
    }
}

Integral { T }  // (2)
class ConstrainedClass {};

/*
auto{T}                                // (4)
auto gcd(T a, T b){
  if( b == 0 ){ return a; }
  else{
    return gcd(b, a % b);
  }
}

auto{T}                                // (5)
class ConstrainedClass{};
*/

int main() {
    auto res = gcd(100, 10);
    ConstrainedClass<int> constrainedClass;
    ConstrainedClass<double> constrainedClass1;  // (3)
    return 0;
}
```

　　I use Template introduction for the function template gcd (line 1) and the class template ConstrainedClass (line 2). As expected,  the concept will kick in if I try to instantiate ConstraintedClass for double.  

　　I don't like it that I can not just replace Integral with auto such as in lines 4 and 5. Up to this point in my posts to concepts, I used constrained placeholders (concepts) and unconstrained placeholders (auto) interchangeably.  

　　Of course, I could easily overcome this restriction by defining a concept that always evaluated to true.  

```c++
#include <iostream>
#include <string>
#include <typeinfo>
#include <utility>

struct NoDefaultConstructor {  // (5)
    NoDefaultConstructor() = delete;
};

template <typename T>  // (1)
concept bool Generic() {
    return true;
}

Generic { T }  // (2)
T gcd(T a, T b) {
    if (b == 0) {
        return a;
    } else {
        return gcd(b, a % b);
    }
}

Generic { T }  // (3)
class ConstrainedClass {
public:
    ConstrainedClass() {
        std::cout << typeid(decltype(std::declval<T>())).name()  // (4)
                  << std::endl;
    }
};

int main() {
    std::cout << "gcd(100, 10): " << gcd(100, 10) << std::endl;
    ConstrainedClass<int> genericClassInt;
    ConstrainedClass<std::string> genericClassString;
    ConstrainedClass<double> genericClassDouble;
    ConstrainedClass<NoDefaultConstructor> genericNoDefaultConstructor;
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// gcd(100, 10): 10
// i
// NSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEE
// d
// 20NoDefaultConstructor
```

　　Generic (line 1) is a concept that returns true for all types. Now, I can unify the syntax and define an unconstrained function template (line 4) and an unconstrained class template (line 3). Honestly, the terms unconstrained or constrained function templates or class template are not official. I coined them for simplicity reasons.  

　　The expression typeid(decltype(std::declval<T>())).name() (line 4)  may look weird to you. std::declvar<T> (C++11) converts the type parameter T into a reference type. Thanks to the reference type, you can use it in a decltype expression to invoke any member function on T without constructing T. It works even for a type T without a default constructor (line 5).   

###  <font color=green>tips: c++面试（四）</font>

#### **【c++面试题】**

##### 1. 请说一说右值和移动
　　表达式分为广义左值和右值。广义左值包括左值和将亡值，右值包括将亡值和纯右值。  

　　纯右值prvalue是没有标识符、不可以取地址的表达式，一般也称为“临时对象”。  

　　c++11开始，c++多了一种引用类型--右值引用。形式是T&& ，它实现了转移语义和完美转发，消除了对象赋值之间不必要的拷贝。  

#### **【数据库面试题】**

##### 1. 请你说一说mysql的四种隔离状态
　　1. 读未提交：一个事务还没提交时，它做的变更就能被其他事务看到。  

　　2. 读提交：一个事务提交后，它做的变更才会被其他事务看到。  

　　3. 可重复读：一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的。当然在可重复读隔离级别下，未提交变更对其他事务也是不可见的。  

　　4. 串行化：顾名思义是对于同一行记录，“写”会加“写锁”，“读”会加“读锁”。当出现读写锁冲突的时候，后访问的事务必须等前一个事务执行完成，才能继续执行。  

事务隔离级别 | 脏读 | 不可重复读 | 幻读
-|-|-|-
读未提交 | 是 | 是 | 是
读提交 | 否 | 是 | 是
可重复读 | 否 | 否 | 是
串行化 | 否 | 否 | 否

##### 2. 请你介绍一下mysql的MVCC机制 
　　MVCC是一种多版本并发控制机制，是MySQL 存储引擎InnoDB实现隔离级别的一种方式。  

　　在可重复读隔离级别下，事务启动的时候就拍了个“快照”，快照是基于整个库的。  

　　InnoDB里面的每个事务都由一个唯一的事务ID，叫做transaction id。每行数据都是有多个版本的，每次事务更新数据的时候，都会生成一个新的数据版本，并且把 transaction id 赋值给这个数据版本的事务 ID，记为 row trx_id。同时，旧的数据版本要保留，并且在新的数据版本中，能够有信息可以直接拿到它。  

　　也就是说，数据表中的一行记录，其实可能有多个版本 (row)，每个版本有自己的 row trx_id。  

　　InnoDB使用到的快照存储在Undo日志中，该日志通过回滚指针把一个数据行所有快照连接起来。  

#### **【设计模式面试题】**

##### 1. 什么是工厂方法？
　　定义一个用于创建对象的接口，让子类决定实例化哪一个类。  

##### 2. 工厂模式有哪几种？
　　简单工厂和工厂方法。  

###  <font color=green>Share: 浅谈适配器模式</font>

#### **【什么是适配器模式？】**
　　《设计模式》一书中是这样定义的：将一个类的接口转换成客户希望的另外一个接口。Adaptor模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。  

　　我们来看下适配器模式的类图：  
![hello](/ARTS2021/ARTS-43/1.png)  

　　图上方是类适配器，采用继承的方式对一个接口与另一个接口进行匹配。  

　　图下方是对象适配器，采用组合的方式进行适配。  

#### **【适配器模式的应用场景】**
　　1. 封装有缺陷的接口设计  

　　2. 统一多个类的接口设计  

　　3. 适配不同格式的数据  

#### **【适配器模式的实现方法】**

##### 1. 类适配器
　　我们看下类适配器的代码实现：  
```c++
#include <iostream>

struct Point {
    Point() : x_(0), y_(0) {}
    Point(int x, int y) : x_(x), y_(y) {}
    int x_;
    int y_;
};

class Shape {
public:
    Shape() = default;
    virtual void BoundingBox(Point& bottom_left, Point& top_right) const {
        bottom_left.x_ = 3;
        bottom_left.y_ = 4;
        top_right.x_ = 5;
        top_right.y_ = 6;
        std::cout << "bottom_left: " << bottom_left.x_ << " " << bottom_left.y_ << std::endl;
        std::cout << "top_right: " << top_right.x_ << " " << top_right.y_ << std::endl;
    }
};

class TextView {
public:
    TextView() = default;
    virtual void GetOrigin(int& x, int& y) const {
        x = 3;
        y = 4;
    }
    virtual void GetExtent(int& x, int& y) const {
        x = 5;
        y = 6;
    }
};

class TextShape : public Shape, private TextView {
public:
    TextShape() = default;
    virtual void BoundingBox(Point& bottom_left, Point& top_right) const {
        int bottom, left, width, height;
        GetOrigin(bottom, left);
        GetExtent(width, height);

        bottom_left = Point(bottom, left);
        top_right = Point(width, height);
        std::cout << "bottom_left: " << bottom_left.x_ << " " << bottom_left.y_ << std::endl;
        std::cout << "top_right: " << top_right.x_ << " " << top_right.y_ << std::endl;
    }
};

int main() {
    TextShape ts;
    Point x;
    Point y;
    ts.BoundingBox(x, y);
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// bottom_left: 3 4
// top_right: 5 6
```
　　需要注意的是，TextShape public继承Shape，private继承TextView，意味着用公共方式继承接口，私有方式继承接口的实现。  

　　TextShape中的BoundingBox方法，对TextView的接口进行转换使之匹配shape的接口。  

##### 2. 对象适配器
　　接着是对象适配器的代码实现：  
```c++
#include <iostream>
#include <memory>

struct Point {
    Point() : x_(0), y_(0) {}
    Point(int x, int y) : x_(x), y_(y) {}
    int x_;
    int y_;
};

class Shape {
public:
    Shape() = default;
    virtual void BoundingBox(Point& bottom_left, Point& top_right) const {
        bottom_left.x_ = 3;
        bottom_left.y_ = 4;
        top_right.x_ = 5;
        top_right.y_ = 6;
        std::cout << "bottom_left: " << bottom_left.x_ << " " << bottom_left.y_ << std::endl;
        std::cout << "top_right: " << top_right.x_ << " " << top_right.y_ << std::endl;
    }
};

class TextView {
public:
    TextView() = default;
    virtual void GetOrigin(int& x, int& y) const {
        x = 3;
        y = 4;
    }
    virtual void GetExtent(int& x, int& y) const {
        x = 5;
        y = 6;
    }
};

class TextShape : public Shape{
public:
    TextShape(const std::shared_ptr<TextView>& textview) : textview_(textview) {}
    virtual void BoundingBox(Point& bottom_left, Point& top_right) const {
        int bottom, left, width, height;
        textview_->GetOrigin(bottom, left);
        textview_->GetExtent(width, height);

        bottom_left = Point(bottom, left);
        top_right = Point(width, height);
        std::cout << "bottom_left: " << bottom_left.x_ << " " << bottom_left.y_ << std::endl;
        std::cout << "top_right: " << top_right.x_ << " " << top_right.y_ << std::endl;
    }

private:
    std::shared_ptr<TextView> textview_;
};

int main() {
    TextShape ts(std::make_shared<TextView>());
    Point x;
    Point y;
    ts.BoundingBox(x, y);
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// bottom_left: 3 4
// top_right: 5 6
```

　　对象适配器采用组合的方式进行接口的适配，可以适配多个接口，更加灵活方便。  

　　TextShape持有一个TextView的指针，对TextView的接口进行转换使之匹配shape的接口。  
