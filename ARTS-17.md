---
layout: post
title: ARST(十七)
date: 2020-08-10 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=blue>Algorithm</font>

#### **【LeetCode:98. Validate Binary Search Tree】**

##### 　　Description:
　　Given a binary tree, determine if it is a valid binary search tree (BST).  
　　Assume a BST is defined as follows:  
　　- The left subtree of a node contains only nodes with keys less than the node's key.  
　　- The right subtree of a node contains only nodes with keys greater than the node's key.  
　　- Both the left and right subtrees must also be binary search trees.  

##### 　　Example 1:
```sh
    2
   / \
  1   3

Input: [2,1,3]
Output: true
```

##### 　　Example 2:
```sh
    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

##### 　　C++ Solution 1:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/098.ValidateBinarySearchTree/ValidateBinarySearchTree1.cpp  

##### 　　C++ Solution 2:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/098.ValidateBinarySearchTree/ValidateBinarySearchTree2.cpp  

###  <font color=blue>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
prone | adj. 俯卧的；有…倾向的，易于…的

#### **【Comments and Conclusions】**
　　<b>99.9% availability - three 9s</b>  

Duration | Acceptable downtime
-|-
Downtime per year | 8h 45min 57s
Downtime per month | 43m 49.7s
Downtime per week | 10m 4.8s
Downtime per day | 1m 26.4s

　　<b>99.99% availability - four 9s</b>  

Duration | Acceptable downtime
-|-
Downtime per year | 52min 35.7s
Downtime per month | 4m 23s
Downtime per week | 1m 5s
Downtime per day | 8.6s


　　<b>Availability in parallel vs in sequence</b>  
　　In sequence:  
　　Overall availability decreases when two components with availability < 100% are in sequence:  
```sh
Availability (Total) = Availability (Foo) * Availability (Bar)
```
　　If both Foo and Bar each had 99.9% availability, their total availability in sequence would be 99.8%.  

　　In parallel:  
　　Overall availability increases when two components with availability < 100% are in parallel:  
```sh
Availability (Total) = 1 - (1 - Availability (Foo)) * (1 - Availability (Bar))
```
　　If both Foo and Bar each had 99.9% availability, their total availability in parallel would be 99.9999%.  

###  <font color=blue>tips: 资深律师老周告诉你必知的法律常识</font>
https://www.bilibili.com/video/BV1GJ411U7gz?t=4216  

　　1.遇到劳动纠纷，首先想方法调解解决问题，不行的话去劳动仲裁。  

　　2.被公司合法裁员，劳动补偿是N+1；被公司非法裁员，劳动赔偿是2N。  

　　3.合同到期时，公司无故不续签，劳动补偿是N+1。  

　　4.劳动仲裁时，要有求证意识。  

###  <font color=blue>Share: boost.test库下的模板模式</font>
　　模板模式：在一个方法中定义一个算法骨架，并将某些步骤推迟到子类中实现。模板方法模式可以让子类在不改变算法整体结构的情况下，重新定义算法中的某些步骤。  

　　许多框架都使用模板模式定义基本的操作步骤，用户只需要实现其中的某些步骤就可以利用框架的所有功能。  

#### **【boost.test库简介】**
　　test库提供了一个用于单元测试的基于命令行界面的测试套件，还附带有检查内存泄漏的功能。它不仅能够支持简单的测试，也能够支持全面的单元测试，并且还具有程序运行监控功能，是一个保证程序正确性的强大工具。  

#### **【测试用例与套件】**
　　test库将测试程序定义为一个测试模块，由测试安装、测试主体、测试清理和测试运行四个部分组成。  

　　测试用例是一个包含多个测试断言的函数，它是可以被独立执行测试的最小单元。  

　　增加测试用例，需要像测试框架注册。我们使用BOOST_AUTO_TEST_CASE像声明函数一样创建测试用例，它的定义是：  
```c++
#define BOOST_AUTO_TEST_CASE(test_name)
```

　　测试套件是测试用例的容器，它包含一个或者多个测试用例，可以将繁多的测试用例分组管理，共享安装/清理代码，更好地组织测试用例。  

　　测试套件跟测试用例一样，使用BOOST_AUTO_TEST_SUITE，它的定义如下：  
```c++
#define BOOST_AUTO_TEST_SUITE(suite_name)
#define BOOST_AUTO_TEST_SUITE_END()
```

　　test框架运用模板模式，将测试用例和测试套件作为扩展点供用户使用。用户使用这些扩展点就可以利用test框架的所有测试功能。  

#### **【boost.test库的使用】**
　　示例代码如下：  
```c++
#define BOOST_TEST_MODULE test_module

#include <boost/test/unit_test.hpp>
#include <iostream>

BOOST_AUTO_TEST_SUITE(test_suite)

BOOST_AUTO_TEST_CASE(case1) { BOOST_CHECK_EQUAL(1, 1); }

BOOST_AUTO_TEST_CASE(case2) { BOOST_CHECK_EQUAL(2, 2); }

BOOST_AUTO_TEST_SUITE_END()
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test -L/var/boost_1_73_0/lib/ -lboost_unit_test_framework -DBOOST_ALL_DYN_LINK
// result:
// Running 2 test cases...
// 
// *** No errors detected
```