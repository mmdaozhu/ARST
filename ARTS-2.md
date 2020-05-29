---
layout: post
title: ARST(二)
date: 2020-04-27 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=red>Algorithm</font>

#### **【LeetCode:169. Majority Element】**

##### 　　Description:
　　Given an array of size n, find the majority element. The majority element is the element that appears more than [n/2] times.
　　You may assume that the array is non-empty and the majority element always exist in the array.

##### 　　Example 1:
```sh
Input: [3,2,3]
Output: 3
```

##### 　　Example 2:
```sh
Input: [2,2,1,1,1,2,2]
Output: 2
```

##### 　　C++ Solution 1:
```cpp
/*
解体思路：
    先排序，然后找中间数，就是众数

时间复杂度分析：
    std::sort是改进后的快排，结果为O(nlogn)
*/

class Solution {
public:
    int majorityElement(std::vector<int>& nums) {
        std::sort(nums.begin(),nums.end());
        return nums[nums.size()/2];
    }
};
```

##### 　　C++ Solution 2:
```cpp
/*
解体思路：
    将数组的数和数的个数以{key,value}的形式存到map里面，然后搜索map，将value最大的key找到

时间复杂度分析：
    map的插入，删除，查找操作的理想状态下的时间复杂度为O(logn);
    vector的遍历为O(n);
    所以结果为O(nlogn);
*/

class Solution {
public:
    int majorityElement(std::vector<int>& nums) {
        std::map<int, int> m;
        for (auto it : nums) {
            if (m.find(it) != m.end()) {
                m[it] += 1;
            } else {
                m[it] = 1;
            }
        }

        int count(0);
        int result(0);
        for (auto it : m) {
            if (it.second > count) {
                count = it.second;
                result = it.first;
            }
        }
        return result;
    }
};
```

###  <font color=red>Review</font>

#### **【Article】**
http://www.aosabook.org/en/distsys.html (1.2. The Basics[Example: Image Hosting Application, Services])

#### **【Words】**

English | Chinese
-|-
proposition | n. 提议；主题；议题
substantial | adj. 大量的；实质的；内容充实的
perceivably | adv. 可知觉地，可察觉地；可领会地
decouple | vt. 解耦
complementary | adj. 互补的
delineation | n. 描述；画轮廓
bottleneck | n. 瓶颈；障碍物
leverage | n. 手段，影响力；杠杆作用；杠杆效率
underlying | adj. 潜在的；根本的；在下面的；优先的
simultaneous | adj. 同时的；联立的；同时发生的
intermingled | adj. 混合的
shards | n. 分片
whereas | conj. 然而；鉴于
outage | n. 中断

#### **【Comments and Conclusions】**
　　When considering scalable system design, it helps to decouple functionality and think about each part of the system as its own service with a clearly defined interface. In practice, systems designed in this way are said to have a SOA.
　　advantage: 1.decouple operations of system. 2.isolate problems.


###  <font color=red>tips：C++ 简单编译命令</font>
```sh
g++ -std=c++11 -Wall -g test.cpp -o test
g++ -std=c++11 -Wall -g -I /var/boost_1_73_0/include/ test.cpp -o test -L/var/boost_1_73_0/lib/ -lboost_regex

```

#### **【选项详解】**
```sh
#使用c++11标准编译
-std=c++11

#一般使用该选项，允许发出GCC能够提供的所有有用的警告
-Wall

#使用-I指定了目录，编译器会先在指定的目录查找头文件，然后再去系统默认头文件目录查找。
-I[dir]

#编译的时候，指定搜索库的路径
-L[dir]

#指定编译的时使用的库
-l[library]  
```

###  <font color=red>Share：C++ Boost Library 的安装和使用</font>

#### **【下载】**
Boost版本：1.73.0
下载链接：https://www.boost.org/users/history/version_1_73_0.html

#### **【环境】**
docker
Linux: Ubuntu-18.04
gcc: 7.4.0 
Boost: 1.73.0

#### **【安装】**
　　大部分Boost libraires是header-only的，不需要编译。
　　少部分Boost libraires是单独编译的，下面是编译这些库的简单步骤。
```sh
cd path/to/boost_1_73_0
./bootstrap.sh --prefix=path/to/installation/prefix
./b2 install
```
	
#### **【使用】**

##### 使用header-only的Boost libraires

　　下面是一个简单的c++程序：
```cpp
#include <boost/lambda/lambda.hpp>
#include <iostream>
#include <iterator>
#include <algorithm>

int main()
{
    using namespace boost::lambda;
    typedef std::istream_iterator<int> in;

    std::for_each(
        in(std::cin), in(), std::cout << (_1 * 3) << " " );
}
```

　　编译：
```sh
g++ -I /var/boost_1_73_0/include/ example.cpp -o example
```
　　运行：
```sh
1 2 3
3 6 9
```

##### 使用separately-compiled的Boost libraires

　　将一个简单的c++程序链接一个boost库：
```cpp
#include <boost/regex.hpp>
#include <iostream>
#include <string>

int main()
{
    std::string line;
    boost::regex pat( "^Subject: (Re: |Aw: )*(.*)" );

    while (std::cin)
    {
        std::getline(std::cin, line);
        boost::smatch matches;
        if (boost::regex_match(line, matches, pat))
            std::cout << matches[2] << std::endl;
    }
}
```

　　编译：
```sh
g++ -I /var/boost_1_73_0/include/ example1.cpp -o example1 -L/var/boost_1_73_0/lib/ -lboost_regex
```
　　运行：
　　1. 先加入库的搜索路径。
```sh
export LD_LIBRARY_PATH=/var/boost_1_73_0/lib/
```

　　2. 执行程序，并贴段字符串：
```sh
To: George Shmidlap
From: Rita Marlowe
Subject: Will Success Spoil Rock Hunter?
---
See subject.
```

　　3. 结果：
```sh
Will Success Spoil Rock Hunter?
```
