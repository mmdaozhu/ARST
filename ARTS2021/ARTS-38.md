---
layout: post
title: ARST(三十八)
date: 2021-01-04 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:122. Best Time to Buy and Sell Stock II】**

##### 　　Description:
　　Say you have an array prices for which the ith element is the price of a given stock on day i.  

　　Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).  

　　Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).  

##### 　　Example 1:
```sh
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```

##### 　　Example 2:
```sh
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```

##### 　　Example 3:
```sh
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

##### 　　C++ Solution:
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/  

###  <font color=yellow>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/concepts-two-extrems-and-the-solution  
　　C++20: Two Extremes and the Rescue with Concepts  

#### **【Words】**
English | Chinese
-|-
biased | adj. 有偏见的，片面的；偏重于……的，偏向……的
diametral | adj. 直径的
decipher | v. 破译，译解；辨认，解释，理解
overwhelming | v. 压倒，淹没，制服
infamous | adj. 声名狼藉的；无耻的；邪恶的；不名誉的
bidirectional | adj. 双向的；双向作用的

#### **【Comments and Conclusions】**

##### Two Extrems
　　Until C++20 we have in C++ two diametral ways to think about functions or classes. Functions or classes can be defined on specific types or on generic types. In the second case, we call them function or class templates. What is wrong with each way?  

##### Too Specific
　　It's quite a job to define for each specific type a function or a class. To avoid that burden, type conversion comes often to our rescue. What seems like rescue is often a curse.  
```c++
#include <iostream>

void needInt(int i) { std::cout << "int: " << i << std::endl; }

int main() {
    std::cout << std::boolalpha << std::endl;

    double d{1.234};
    std::cout << "double: " << d << std::endl;
    needInt(d);

    std::cout << std::endl;
    bool b{true};
    std::cout << "bool: " << b << std::endl;
    needInt(b);
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// 
// double: 1.234
// int: 1
// 
// bool: true
// int: 1
```

　　Invoking needInt(int i) with a double gives you narrowing conversion.  

　　Invoking needInt(int i) with a bool promotes the bool to int.  

　　My strong belief is that we need for convenience reasons the entire magic of conversions in C/C++ to deal with the fact that functions only accept specific types.  

　　Okay. Let's do it the other way around. Write not specific, but write generically. Maybe, writing generic code with templates is our rescue.  

##### Too Generic
　　Here is my first try. Sorting is such a generic idea. It should work for each container if the elements of the container are sortable. Let's apply std::sort to a std::list.  
```c++
#include <algorithm>
#include <list>

int main() {
    std::list<int> myList{1, 10, 3, 2, 5};
    std::sort(myList.begin(), myList.end());
}
```

　　This is, what you get when I try to compile the small program.  
```sh
In file included from /usr/local/include/c++/10.2.0/algorithm:62,
                 from test.cpp:1:
/usr/local/include/c++/10.2.0/bits/stl_algo.h: In instantiation of 'constexpr void std::__sort(_RandomAccessIterator, _RandomAccessIterator, _Compare) [with _RandomAccessIterator = std::_List_iterator<int>; _Compare = __gnu_cxx::__ops::_Iter_less_iter]':
/usr/local/include/c++/10.2.0/bits/stl_algo.h:4859:18:   required from 'constexpr void std::sort(_RAIter, _RAIter) [with _RAIter = std::_List_iterator<int>]'
test.cpp:6:43:   required from here
/usr/local/include/c++/10.2.0/bits/stl_algo.h:1975:22: error: no match for 'operator-' (operand types are 'std::_List_iterator<int>' and 'std::_List_iterator<int>')
 1975 |     std::__lg(__last - __first) * 2,
      |               ~~~~~~~^~~~~~~~~
In file included from /usr/local/include/c++/10.2.0/bits/stl_algobase.h:67,
                 from /usr/local/include/c++/10.2.0/algorithm:61,
                 from test.cpp:1:
/usr/local/include/c++/10.2.0/bits/stl_iterator.h:500:5: note: candidate: 'template<class _IteratorL, class _IteratorR> constexpr decltype ((__y.base() - __x.base())) std::operator-(const std::reverse_iterator<_IteratorL>&, const std::reverse_iterator<_IteratorR>&)'
  500 |     operator-(const reverse_iterator<_IteratorL>& __x,
```

　　What is going wrong? Let's have a closer look at the signature of the used overload of std::sort.  
```c++
template <class RandomIt>
void sort(RandomIt first, RandomIt last);
```

　　std::sort uses strange-named arguments such as RandomIt. RandomIt stands for a random access iterator. This is the reason for the overwhelming error message, for which templates are infamous. A std::list provides only a bidirectional iterator but std:sort requires a random access iterator. The structure of a std::list makes this obvious.  

##### Concepts to the Rescue
　　Concepts are the rescue because they put semantic constraints on template parameter.  

　　Here are the already mentioned type requirements on std::sort.  

　　- RandomIt must meet the requirements of ValueSwappable and LegacyRandomAccessIterator.  
　　- The type of dereferenced RandomIt must meet the requirements of MoveAssignable and MoveConstructible.  
　　- Compare must meet the requirements of Compare.  

　　The type requirements on std::sort are concepts. For a short introduction to concepts, read my post C++20: The Big Four. In particular, std::sort requires a LegacyRandomAccessIterator. Let's have a closer look at the concept. I polished the example from cppreference.com a little bit.  
```c++
template <typename It>
concept LegacyRandomAccessIterator = LegacyBidirectionalIterator<It>&&  // (1)
    std::totally_ordered<It>&& requires(It i, typename std::incrementable_traits<It>::difference_type n) {
    { i += n }
    ->std::same_as<It&>; // (2)
    { i -= n }
    ->std::same_as<It&>;
    { i + n }
    ->std::same_as<It>;
    { n + i }
    ->std::same_as<It>;
    { i - n }
    ->std::same_as<It>;
    { i - i }
    ->std::same_as<decltype(n)>;
    { i[n] }
    ->std::convertible_to<std::iter_reference_t<It>>;
};
```

　　Here is the key observation. A type It supports the concept LegacyRandomAccessIterator if it supports the concept LegacyBidirectionalIterator and all other requirements. For example, the requirement in line 2 means that for a value of type It: { i += n } is a valid expression and it returns an I&. To complete my story, std::list support a LegacyBidirectionalIterator.  

　　Admittedly, this section was quite technical. Let's try it out. With concepts, you could expect a concise error message such as the following on:  
![hello](/ARTS2021/ARTS-38/1.png)  

　　Of course, this error message was a fake, because no compiler implements the C++20 syntax for concepts.  

##### The long, long History
　　I heard the first time about concepts around 2005 - 2006. The reminded me of Haskell type classes. Type classes in Haskell are interfaces for similar types.  

　　But C++ concepts are different. Here are a few observations.  

　　- In Haskell, a type has to be an instance of a type class. In C++20, a type has to fulfil the requirements of a concept.  
　　- Concepts can be used on non-type arguments of templates. For example, numbers such as 5 are non-type arguments. When you want to have a std::array of int's with 5 elements, you use the non-type argument 5: std::array<int, 5> myArray.  
　　- Concepts add no run-time costs.  

###  <font color=yellow>tips: 优秀架构师是如何学习开源项目的</font>
https://www.bilibili.com/video/BV13D4y1Q7AY  

##### 方法1
　　简单看源码：简单看源码，文档，跑跑样例代码  

　　产出+闭环：否  

　　Get技能：源码阅读技能  

　　案例：简单小项目：https://github.com/Jaskey/ConsistentHash  


##### 方法2
　　整理源码：将所有源代码整理一遍，包括将包名都改掉，编译/测试/运行通过  

　　产出+闭环：否  

　　Get技能：细粒度源码阅读+调试  

　　案例：波波看源码习惯，案例spring-petclinic  
　　　　　　https://github.com/spring-projects/spring-petclinic  
　　　　　　https://github.com/spring2go/spring-petclinic-mono  


##### 方法3
　　整理+输出+分享：源码整理过，并且总结成源码分析或者架构设计文档/ppt，在公司或者社区分享  

　　产出+闭环：有价值的输出+反馈  

　　Get技能：技术/整理/输出/分享技能  

　　案例：波波看源码习惯，案例spring-petclinic  
　　　　　　https://github.com/spring-projects/spring-petclinic  
　　　　　　https://github.com/spring2go/spring-petclinic-mono  
　　　　　　https://www.bilibili.com/video/BV1o5411x7ML  
　　　　　　PPD新手工程师每月源码解析文章  

##### 方法4
　　开发克隆版：开发原项目的克隆版，类似翻译，比方说原项目用go开发，我克隆成java版，并开源分享到社区  

　　产出+闭环：有价值的产出+社区反馈  

　　Get技能：深度源码阅读，技术/项目开发技能，find high-quality projects on github  

　　案例：Staffjoy项目：分享：Spring Boot 与 Kubernets云原生微服务实践  
　　　　　　https://github.com/Staffjoy/v2  
　　　　　　https://github.com/spring2go/staffjoy  
　　　　　　Kafka克隆项目：  https://github.com/adyliu/jafka  
　　　　　　https://github.com/travisjeffery/jocko  
　　　　　　https://github.com/bulldog2011/luxun  

##### 方法5
　　生成化落地：源码经过整理输出，并且在公司生产级项目中落地，承担业务流量，并且根据业务需要进行定制或者自研  

　　产出+闭环：有价值的产出+用户反馈+持续改进  

　　Get技能：项目实战落地技能，企业级源码阅读和定制技能  

　　案例：携程Zuul网官：分享：微服务架构实战160讲 https://github.com/spring2go/s2g-zuul   
　　　　　　携程Hystrix限流熔断 分享：微服务架构实战160讲 https://github.com/ctripcorp/chystrix.net    
　　　　　　拍拍贷PMQ消息系统  https://github.com/ppdaicorp/pmq   
　　　　　　饿了么监控应用系统：  https://qcon.infoq.cn/2019/shanghai/presentation/1902 https://myslide.cn/slides/21832  

##### 方法6
　　开发知识产品：将开源项目代码完全吸收，开发克隆项目/或开发开源应用演示项目，并制作成知识付费产品（视频/书籍），发布到商业平台。  

　　产出+闭环：有价值的产出+用户反馈+持续改进  

　　Get技能：技术和项目开发技能，知识产品设计和开发技能，行业影响力。  

　　案例：Staffjoy项目：Spring Boot 与 Kubernets云原生微服务实践  https://github.com/spring2go/staffjoy  
　　　　　　新峰商城：https://github.com/newbee-ltd/newbee-mall  


##### 方法7
　　开发自己的开源项目： 在吸收开源项目+企业落地实践的基础上，沉淀出自己的开源项目，并持续推动开源社区建设  

　　产出+闭环：有产品输出+社区反馈+持续改进  

　　Get技能：技术和项目开发技能，行业影响力，跳槽最佳姿势，开源和社区运营技能  

　　案例：Apollo： https://github.com/ctripcorp/apollo  
　　　　　　Skywalking： https://github.com/apache/skywalking  
　　　　　　LinDB： https://github.com/lindb/lindb  

##### 方法8
　　商业化自己的开源项目： 在自己的开源产品基础上，持续推进社区生态建设，并逐步走上商业化服务的道路  

　　产出+闭环：完全和企业客户闭环  

　　Get技能：产品化和商业化技能/社区生态建设技能/技术领导力  

　　案例：TiDB： https://github.com/pingcap/tidb  
　　　　　　Kafka： https://github.com/apache/kafka  
　　　　　　Sentry： https://github.com/getsentry/sentry  

##### 要点心得
　　1. 输入要有输出，学习要有产出。  
　　2. 闭环反馈+持续改进，用户越多，反馈越多，学习效果越好。最好你的学习效果能够量化  
　　3. 如果你要真正理解某人/事物，那么就尝试改变他/她/它，学习开源项目也一样  
　　4. 一举多得，投入时间->有价值产出->个人沉淀积累->贡献社区->提升影响力->真正学到东西  


###  <font color=yellow>Share: 高性能缓存架构</font>

#### **【缓存的架构设计要点】**
　　缓存虽然能够大大减轻存储系统的压力，但同时也给架构引入了更多复杂性。架构设计时如果没有针对缓存的复杂性进行处理，某些场景下甚至会导致整个系统崩溃。缓存的架构设计要点包括缓存穿透、
缓存雪崩和缓存热点。  

#### **【缓存穿透】**
　　缓存穿透是指缓存没有发挥作用，业务系统虽然去缓存查询数据，但缓存中没有数据，业务系统需要再次去存储系统查询数据。通常情况下有两种情况：  

##### 1. 存储数据不存在
　　被访问的数据确实不存在。缓存在这个场景中并没有起到分担存储系统访问压力的作用。  

　　这种情况的解决办法比较简单，如果查询存储系统的数据没有找到，则直接设置一个默认值（可以是空值，也可以是具体的值）存到缓存中，这样第二次读取缓存时就会获取到默认值，而不会继续访问存储系统。  

##### 2. 缓存数据生成耗费大量时间或者资源
　　存储系统中存在数据，但生成缓存数据需要耗费较长时间或者耗费大量资源。  

　　典型的业务场景就是电商的商品分页。因此业务上最简单的就是每次点击分页的时候按分页计算和生成缓存。  

#### **【缓存雪崩】**
　　缓存雪崩是指当缓存失效（过期）后引起系统性能急剧下降的情况。  

　　当缓存过期被清除后，业务系统需要重新生成缓存，因此需要再次访问存储系统，再次进行运算，这个处理步骤耗时几十毫秒甚至上百毫秒。而对于一个高并发的业务系统来说，几百毫秒内可能会接到几百上千个请求。由于旧的缓存已经被清除，新的缓存还未生成，并且处理这些请求的线程都不知道另外有一个线程正在生成缓存，因此所有的请求都会去重新生成缓存，都会去访问存储系统，从而对存储系统造成巨大的性能压力。  

　　缓存雪崩的常见解决方法有两种：更新锁机制和后台更新机制。  

##### 1. 更新锁
　　对缓存更新操作进行加锁保护，保证只有一个线程能够进行缓存更新，未能获取更新锁的线程要么等待锁释放后重新读取缓存，要么就返回空值或者默认值。  

##### 2. 后台更新
　　由后台线程来更新缓存，而不是由业务线程来更新缓存，缓存本身的有效期设置为永久，后台线程定时更新缓存。  

　　后台定时机制需要考虑一种特殊的场景，当缓存系统内存不够时，会“踢掉”一些缓存数据，从缓存被“踢掉”到下一次定时更新缓存的这段时间内，业务线程读取缓存返回空值，而业务线程本身又不会去更新缓存，因此业务上看到的现象就是数据丢了。解决的方式有两种：  

　　1. 后台线程除了定时更新缓存，还要频繁地去读取缓存（例如，1 秒或者 100 毫秒读取一次），如果发现缓存被“踢了”就立刻更新缓存，这种方式实现简单，但读取时间间隔不能设置太长。  

　　2. 业务线程发现缓存失效后，通过消息队列发送一条消息通知后台线程更新缓存。可能会出现多个业务线程都发送了缓存更新消息，但其实对后台线程没有影响，后台线程收到消息后更新缓存前可以判断缓存是否存在，存在就不执行更新操作。这种方式实现依赖消息队列，复杂度会高一些，但缓存更新更及时，用户体验更好。  

#### **【缓存热点】**
　　虽然缓存系统本身的性能比较高，但对于一些特别热点的数据，如果大部分甚至所有的业务请求都命中同一份缓存数据，则这份数据所在的缓存服务器的压力也很大。  

　　缓存热点的解决方案就是复制多份缓存副本，将请求分散到多个缓存服务器上，减轻缓存热点导致的单台缓存服务器压力。  

　　缓存副本设计有一个细节需要注意，就是不同的缓存副本不要设置统一的过期时间，否则就会出现所有缓存副本同时生成同时失效的情况，从而引发缓存雪崩效应。正确的做法是设定一个过期时间范围，不同的缓存副本的过期时间是指定范围内的随机值。  

#### **【实现方式】**
　　由于缓存的各种访问策略和存储的访问策略是相关的，因此上面的各种缓存设计方案通常情况下都是集成在存储访问方案中，可以采用“程序代码实现”的中间层方式，也可以采用独立的中间件来实现。  

#### **【脑图】**
![hello](/ARTS2021/ARTS-38/2.png)  
