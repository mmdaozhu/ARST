---
layout: post
title: ARST(四十一)
date: 2021-01-25 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:309. Best Time to Buy and Sell Stock with Cooldown】**

##### 　　Description:
　　You are given an array prices where prices[i] is the price of a given stock on the ith day.  

　　Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:  

　　After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).  

　　Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).  

##### 　　Example 1:
```sh
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

##### 　　Example 2:
```sh
Input: prices = [1]
Output: 0
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/309.BestTimeToBuyAndSellStockWithCooldown/BestTimeToBuyAndSellStockWithCooldown.cpp  

###  <font color=yellow>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/c-20-concepts-the-placeholder-syntax  
　　C++20: Concepts, the Placeholder Syntax  

#### **【Words】**
English | Chinese
-|-
detour | n. 迂回路；临时绕行道路
asymmetrie | n. 不对称
seminars | n. 研讨会（seminar的复数）；专题讨论会
disjunct | adj. 分离的

#### **【Comments and Conclusions】**

##### From Asymmetries in C++11/14 to Symmetries in C++20
　　I often have discussions in my C++ seminars which goes like this.  

　　What is the Standard Template Library from the birds-eyes perspective? Generic containers which can be manipulated with generic algorithms. The glue between these two disjunct components are iterators.  

　　Generic containers and generic algorithms mean that they are not bound to a specific type. Fine. Many of the algorithms can be parametrised by a callable. For example, std::sort has an overload which takes a binary predicate (callable). This binary predicate is applied to the elements of the container. A binary predicate is a function which takes two arguments and returns a boolean. You can use a function, a function object, or with C++11 a lambda as a binary predicate.  

##### The Small Asymmetry in C++11
　　What's wrong with the following program?  
```c++
#include <algorithm>
#include <array>
#include <iostream>
#include <string>
#include <vector>

template <typename Cont, typename Pred>
void sortDescending(Cont& cont, Pred pred) {
    std::sort(cont.begin(), cont.end(), pred);
}

template <typename Cont>
void printMe(const Cont& cont) {
    for (auto c : cont) {
        std::cout << c << " ";
    }
    std::cout << std::endl;
}

int main() {
    std::array<int, 10> myArray{5, -10, 3, 2, 7, 8, 9, -4, 3, 4};
    std::vector<double> myVector{5.1, -10.5, 3.1, 2.0, 7.2, 8.3};
    std::vector<std::string> myVector2{"Only", "for", "testing", "purpose"};

    sortDescending(myArray, [](int fir, int sec) { return fir > sec; });            // (1)
    sortDescending(myVector, [](double fir, double sec) { return fir > sec; });     // (2)
    sortDescending(myVector2, [](const std::string& fir, const std::string& sec) {  // (3)
        return fir > sec;
    });

    printMe(myArray);
    printMe(myVector);
    printMe(myVector2);
    return 0;
}
```
　　Although containers and algorithms are generic, C++11 has only type-based lambdas. I have to implement the binary predicate for each data type (lines 1 - 3) and break, therefore, my generic approach.  

　　With C++14, this asymmetry disappears. Containers, algorithms are generic and lambdas can be generic.  

##### The Big Asymmetry in C++14
　　C++14 makes the implementation of the previous program.  
```c++
#include <algorithm>
#include <array>
#include <iostream>
#include <string>
#include <vector>

template <typename Cont>
void sortDescending(Cont& cont) {
    std::sort(cont.begin(), cont.end(), [](auto fir, auto sec) {  // (1)
        return fir > sec;
    });
}

template <typename Cont>
void printMe(const Cont& cont) {
    for (auto c : cont) {
        std::cout << c << " ";
    }
    std::cout << std::endl;
}

int main() {
    std::array<int, 10> myArray{5, -10, 3, 2, 7, 8, 9, -4, 3, 4};
    std::vector<double> myVector{5.1, -10.5, 3.1, 2.0, 7.2, 8.3};
    std::vector<std::string> myVector2{"Only", "for", "testing", "purpose"};

    sortDescending(myArray);    // (2)
    sortDescending(myVector);   // (2)
    sortDescending(myVector2);  // (2)

    printMe(myArray);
    printMe(myVector);
    printMe(myVector2);
    return 0;
}
```
　　Because I can use a generic lambda (line 1) for each data-type in line 2. No, because I replaced one small asymmetry in C++11 with a bigger asymmetry in C++14. The small asymmetry in C++11 was, that lambdas are type-bound. The big asymmetry in C++14 is, that generic lambdas introduced a new syntactic form to write generic functions, better known as function templates.  

　　Here is the proof:  
```c++
#include <iostream>
#include <string>

auto addLambda = [](auto fir, auto sec) { return fir + sec; };  // (1)

template <typename T, typename T2>  // (2)
auto addTemplate(T fir, T2 sec) {
    return fir + sec;
}

int main() {
    std::cout << std::boolalpha << std::endl;

    std::cout << addLambda(1, 5) << " " << addTemplate(1, 5) << std::endl;
    std::cout << addLambda(true, 5) << " " << addTemplate(true, 5) << std::endl;
    std::cout << addLambda(1, 5.5) << " " << addTemplate(1, 5.5) << std::endl;

    const std::string fir{"ge"};
    const std::string sec{"neric"};
    std::cout << addLambda(fir, sec) << " " << addTemplate(fir, sec) << std::endl;
    return 0;
}
```
　　This is the asymmetry in C++14. Generic lambdas introduce a new way to define function templates.  

　　Can we use auto in functions to get function templates? No with C++17 but yes with C++20.  In C++20, you can use constrained placeholders (concepts) or unconstrained placeholders (auto) in function declarations to get function templates.  

##### The Solution in C++20
　　Before I write about the new way to define function templates, I want to answer my origin question: Where can I use my concept? Concepts can be used where auto is usable.  
```c++
#include <iostream>
#include <type_traits>
#include <vector>

template <typename T>  // (1)
concept Integral = std::is_integral<T>::value;

Integral auto getIntegral(int val) {  // (2)
    return val;
}

int main() {
    std::cout << std::boolalpha << std::endl;

    std::vector<int> vec{1, 2, 3, 4, 5};
    for (Integral auto i : vec) std::cout << i << " ";  // (3)

    Integral auto b = true;  // (4)
    std::cout << b << std::endl;

    Integral auto integ = getIntegral(10);  // (5)
    std::cout << integ << std::endl;

    auto integ1 = getIntegral(10);  // (6)
    std::cout << integ1 << std::endl;
    return 0;
}
```

　　The concept Integral in line 1 can be used as a return type (line 2), in a range-based for-loop (line 3), or as a type for the variable b (line 4) or the variable integ (line 5). To see the symmetry, line 6 uses type-deduction with auto instead. I have to admit that there is now no compiler available, which can compile completely the proposed concepts syntax which I used in my example.  

###  <font color=yellow>tips: c++面试（二）</font>

#### **【c++笔试题】**
　　1. 以数组名作函数参数时,实参数组与形参数组都不必定义长度,因此实参与形参的结合方式是地址结合,与数组长度无关。请问这句话的说法是正确的吗？  

　　A. 正确  
　　B. 错误  

　　答案：B（二维数组声明为形参的时候，必须要保留第二维的大小，当使用指针的时候，需要用数组指针）  

　　2. 以下哪个是带行缓冲的IO  
```c++
A. write(STDOUT_FILENO, "helloworld", 10); // 不带缓存
B. fprintf(stderr, "helloworld"); //不带缓冲
C. fwrite("helloworld", 10, 1, stdout); // 行缓冲 
D. fo = fopen("a.txt", "w"); fwrite("helloworld", 10, 1, fo); // 全缓冲
```

　　答案：C  

　　3. 以下哪个函数可以在源地址和目的地址的位置任意的情况下，在源地址和目的地址的空间大小任意的情况下实现二进制代码块的复制？  

　　A. memcpy()  
　　B. memmove()  
　　C. memset()  
　　D. strcpy()  

　　答案：B  

#### **【c++面试题】**

##### 1. C++ STL中的map有几种？用什么实现的？
　　map、unordered_map、multimap、unordered_multimap  

　　map、multimap底层是由红黑树实现的，红黑树是一种二叉搜索树，查询插入删除的时间复杂度是O(logn)的。它是一种弱平衡的，性能比AVL高。  

　　unordered_map、unordered_multimap底层是由Hash表实现的，查询插入删除的时间复杂度是O(1)的。  

##### 2. C++ 中的虚函数是如何实现运行时多态的？
　　带有虚函数的类对象中都有一个虚函数表指针。当派生类重写基类的虚函数时，该虚函数表中的函数地址就会被替换。当子类的指针或对象赋给父类的指针或引用时，父类对象的虚函数表指针就会替换成子类的，从而实现了运行时多态。  

#### **【数据库面试题】**

##### 1. 请你说一说数据库索引
　　数据库索引是为了增加查询速度而对表字段附加的一种标识，是对数据库中一列或多列的值进行排序的一种数据结构。  

　　数据库在执行SQL语句的时候，默认的方式是根据搜索条件进行全表扫描，遇到匹配条件的就加入搜索结果集合。如果我们对某一字段增加索引，查询时就会先去索引列表中一次定位到特定值的行数，增大大减少遍历匹配的行数加了查询的速度。  

##### 2. 请你说一说数据库事务
　　数据库事务是指一组原子性SQL操作，要么全部成功，要么全部失败。  

　　数据库事务满足ACID属性。  

　　Atomicity（原子性）：原子性指事务包含的所有的操作要么全部成功，要么全部失败回滚。  

　　Consistency（一致性）：一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，事务开始前或者结束后必须处于一致性状态。  

　　Isolation（隔离性）：隔离性是当多个用户并发访问数据库时，每个用户开启的事务不能被其他事务的操作所干扰，多个并发事务之间要互相隔离。  

　　Durability（持久性）：持久性时指一个事务一旦被提交了，那么对数据库中的数据的改变就是持久的。  

#### **【设计模式面试题】**

##### 1. 什么是单例模式？
　　单例模式：单例模式指一个类只能创建一个实例，并提供一个访问它的全局访问点。  

　　单例模式一般用于配置全局类、ID生成器等等。  

##### 2. 单例模式有哪几种实现方式
　　单例模式有饿汉式和懒汉式。  

###  <font color=yellow>Share: 高性能负载均衡：分类及架构</font>
　　<b>高性能集群的复杂性主要体现在需要增加一个任务分配器，以及为任务选择一个合适的任务分配算法。</b>  

　　实际上任务分配并不只是考虑计算单元的负载均衡，不同的任务分配算法目标是不一样的，有的基于负载考虑，有的基于性能（吞吐量、响应时间）考虑，有的基于业务考虑。<b>负载均衡不只是为了计算单元的负载达到均衡状态。</b>  

#### **【DNS 负载均衡】**
　　DNS 是最简单也是最常见的负载均衡方式，一般用来实现地理级别的均衡。  

　　下面是 DNS 负载均衡的简单示意图：  
![hello](/ARTS2021/ARTS-41/1.jpg)  

　　DNS 负载均衡实现简单、成本低，但也存在粒度太粗、负载均衡算法少等缺点。仔细分析一下优缺点，其优点有：  

　　1. 简单、成本低：负载均衡工作交给 DNS 服务器处理，无须自己开发或者维护负载均衡设备。  
　　2. 就近访问，提升访问速度：DNS 解析时可以根据请求来源 IP，解析成距离用户最近的服务器地址，可以加快访问速度，改善性能。  

　　缺点有：  

　　1. 更新不及时：DNS 缓存的时间比较长，修改 DNS 配置后，由于缓存的原因，还是有很多用户会继续访问修改前的 IP，这样的访问会失败，达不到负载均衡的目的，并且也影响用户正常使用业务。  
　　2. 扩展性差：DNS 负载均衡的控制权在域名商那里，无法根据业务特点针对其做更多的定制化功能和扩展特性。  
　　3. 分配策略比较简单：DNS 负载均衡支持的算法少；不能区分服务器的差异（不能根据系统与服务的状态来判断负载）；也无法感知后端服务器的状态。  

　　针对 DNS 负载均衡的一些缺点，对于时延和故障敏感的业务，有一些公司自己实现了 HTTP-DNS 的功能，即使用 HTTP 协议实现一个私有的 DNS 系统。这样的方案和通用的 DNS 优缺点正好相反。  

#### **【硬件负载均衡】**
　　硬件负载均衡是通过单独的硬件设备来实现负载均衡功能，这类设备和路由器、交换机类似，可以理解为一个用于负载均衡的基础网络设备。目前业界典型的硬件负载均衡设备有两款：F5 和 A10。  

　　硬件负载均衡的优点是：  

　　1. 功能强大：全面支持各层级的负载均衡，支持全面的负载均衡算法，支持全局负载均衡。  
　　2. 性能强大：对比一下，软件负载均衡支持到 10 万级并发已经很厉害了，硬件负载均衡可以支持 100 万以上的并发。  
　　3. 稳定性高：商用硬件负载均衡，经过了良好的严格测试，经过大规模使用，稳定性高。  
　　4. 支持安全防护：硬件均衡设备除具备负载均衡功能外，还具备防火墙、防 DDoS 攻击等安全功能。  

　　硬件负载均衡的缺点是：  

　　1. 价格昂贵：最普通的一台 F5 就是一台“马 6”，好一点的就是“Q7”了。  
　　2. 扩展能力差：硬件设备，可以根据业务进行配置，但无法进行扩展和定制。  

#### **【软件负载均衡】**
　　软件负载均衡通过负载均衡软件来实现负载均衡功能，常见的有 Nginx 和 LVS，其中 Nginx 是软件的 7 层负载均衡，LVS 是 Linux 内核的 4 层负载均衡。  

　　软件和硬件的最主要区别就在于性能，硬件负载均衡性能远远高于软件负载均衡性能。  

　　Nginx 的性能是万级，一般的 Linux 服务器上装一个 Nginx 大概能到 5 万 / 秒；LVS 的性能是十万级，据说可达到 80 万 / 秒；而 F5 性能是百万级，从 200 万 / 秒到 800 万 / 秒都有。  

　　下面是 Nginx 的负载均衡架构示意图：  
![hello](/ARTS2021/ARTS-41/2.jpg)  

　　软件负载均衡的优点：  

　　1. 简单：无论是部署还是维护都比较简单。  
　　2. 便宜：只要买个 Linux 服务器，装上软件即可。  
　　3. 灵活：4 层和 7 层负载均衡可以根据业务进行选择；也可以根据业务进行比较方便的扩展，例如，可以通过 Nginx 的插件来实现业务的定制化功能。  

　　其实下面的缺点都是和硬件负载均衡相比的，并不是说软件负载均衡没法用。  

　　1. 性能一般：一个 Nginx 大约能支撑 5 万并发。  
　　2. 功能没有硬件负载均衡那么强大。  
　　3. 一般不具备防火墙和防 DDoS 攻击等安全功能。  

#### **【负载均衡典型架构】**
　　每种方式都有一些优缺点，但并不意味着在实际应用中只能基于它们的优缺点进行非此即彼的选择，反而是基于它们的优缺点进行组合使用。  

　　具体来说，组合的基本原则为：DNS 负载均衡用于实现地理级别的负载均衡；硬件负载均衡用于实现集群级别的负载均衡；软件负载均衡用于实现机器级别的负载均衡。  

　　我以一个假想的实例来说明一下这种组合方式，如下图所示。  
![hello](/ARTS2021/ARTS-41/3.png)  

　　需要注意的是，上图只是一个示例，一般在大型业务场景下才会这样用，如果业务量没这么大，则没有必要严格照搬这套架构。  

#### **【脑图】**
![hello](/ARTS2021/ARTS-41/4.png)  

