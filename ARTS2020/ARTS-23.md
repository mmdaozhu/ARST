---
layout: post
title: ARST(二十三)
date: 2020-09-21 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=indigo>Algorithm</font>

#### **【LeetCode:121. Best Time to Buy and Sell Stock】**
　　Say you have an array for which the ith element is the price of a given stock on day i.  

　　If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.  

　　Note that you cannot sell a stock before you buy one.  

##### 　　Example 1:
```sh
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

##### 　　Example 2:
```sh
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/121.BestTimeToBuyAndSellStock/BestTimeToBuyAndSellStock.cpp  

###  <font color=indigo>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
concurrently | adv. 兼；同时发生地
serially | adv. 连续地；连载地
provision | vt. 供给…食物及必需品
coordinate | v. 调节，配合
federation | n. 联合；联邦
lag | n. 落后；迟延；

#### **【Comments and Conclusions】**

##### Relational database management system (RDBMS)
　　A relational database like SQL is a collection of data items organized in tables.  

##### ACID
　　ACID is a set of properties of relational database transactions.  

　　- Atomicity - Each transaction is all or nothing  
　　- Consistency - Any transaction will bring the database from one valid state to another  
　　- Isolation - Executing transactions concurrently has the same results as if the transactions were executed serially  
　　- Durability - Once a transaction has been committed, it will remain so  

##### Master-slave replication
　　The master serves reads and writes, replicating writes to one or more slaves, which serve only reads. Slaves can also replicate to additional slaves in a tree-like fashion. If the master goes offline, the system can continue to operate in read-only mode until a slave is promoted to a master or a new master is provisioned.  

　　Disadvantage(s): Additional logic is needed to promote a slave to a master.  

##### Master-master replication
　　Both masters serve reads and writes and coordinate with each other on writes. If either master goes down, the system can continue to operate with both reads and writes.  

　　Disadvantage(s):  
　　- You'll need a load balancer or you'll need to make changes to your application logic to determine where to write.  
　　- Most master-master systems are either loosely consistent (violating ACID) or have increased write latency due to synchronization.  
　　- Conflict resolution comes more into play as more write nodes are added and as latency increases.  

##### federation
　　Federation (or functional partitioning) splits up databases by function. For example, instead of a single, monolithic database, you could have three databases: forums, users, and products, resulting in less read and write traffic to each database and therefore less replication lag. Smaller databases result in more data that can fit in memory, which in turn results in more cache hits due to improved cache locality. With no single central master serializing writes you can write in parallel, increasing throughput.  

　　Disadvantage(s):  
　　- Federation is not effective if your schema requires huge functions or tables.  
　　- You'll need to update your application logic to determine which database to read and write.  
　　- Joining data from two databases is more complex with a server link.  
　　- Federation adds more hardware and additional complexity.  
　
###  <font color=indigo>tips: 直面问题，咱谈焦虑，谈烦恼，谈怎么成长</font>
https://www.bilibili.com/video/BV1C7411F7x3  

##### 1. 面对焦虑，认识自我

###### 技术人员典型的焦虑和烦恼
　　- 加班：劳动时间过长，没有时间提升  
　　- 搬砖：工作没有技术含量  
　　- 成长：瓶颈期  
　　- 学习：东西太多学不过来  
　　- 彷徨：失去方向，未来要干什么，要做什么不知道  

###### 认识一下这个世界
　　这个世界是怎么组成的？  
　　- 基础技术、工具、产品、项目  
　　- 大家的分工是怎么来的？  

　　这个世界需要什么样的人？以及这些人的特点  
　　- 劳工、技工、特种工、设计、架构、经理  
　　- Google评分卡  

　　这个世界的技术趋势和规律是什么样的？  
　　- 工业化革命、信息化革命  
　　- 技术更新淘汰、风口是什么样的  

###### Google评分卡
　　0 - you are unfamiliar with the subject area.  

　　1 - you can read / understand the most fundamental aspects of the subject area.  

　　2 - ability to implement small changes, understand basic principles and able to figure out additional details with minimal help.  

　　3 - basic proficiency in a subject area without relying on help.  

　　4 - you are comfortable with the subject area and all routine work on it:For software areas - ability to develop medium programs using all basic language features w/o book, awareness of more esoteric features (with book).For systems areas - understanding of many fundamentals of networking and systems administration, ability to run a small network of systems including recovery, debugging and nontrivial troubleshooting that relies on the knowledge of internals.  

　　5 - an even lower degree of reliance on reference materials. Deeper skills in a field or specific technology in the subject area.  

　　6 - ability to develop large programs and systems from scratch. Understanding of low level details and internals. Ability to design / deploy most large, distributed systems from scratch.  

　　7 - you understand and make use of most lesser known language features, technologies, and associated internals. Ability to automate significant amounts of systems administration.  

　　8 - deep understanding of corner cases, esoteric features, protocols and systems including “theory of operation”. Demonstrated ability to design, deploy and own very critical or large infrastructure, build accompanying automation.  

　　9 - could have written the book about the subject area but didn’t; works with standards committees on defining new standards and methodologies.  

　　10 - wrote the book on the subject area (there actually has to be a book). Recognized industry expert in the field, might have invented it.  

　　Subject Areas:  
　　- TCP/IP Networking (OSI stack, DNS etc)  
　　- Unix/Linux internals  
　　- Unix/Linux Systems administration  
　　- Algorithms and Data Structures  
　　- C  
　　- C++  
　　- Python  
　　- Java  
　　- Perl  
　　- Go  
　　- Shell Scripting (sh, Bash, ksh, csh)  
　　- SQL and/or Database Admin  
　　- Scripting language of your choice (not already mentioned)  
　　- People Management  
　　- Project Management  

###### 认识自己
　　- 自己的喜好：找到自己可以坚持不会放弃的东西  
　　- 自己的特长是什么：1.找到自己可以干成的事 2.找到别人会来请教你的事  

##### 2. 打实基础，一通百通

###### 为什么要学习基础技术
　　一通百通  
　　- 所有的技术原理和本质都在基础技术上  

　　突破瓶颈  
　　- 只有基础技术才能让你上升到更高的层次  
　　- 在技术世界里，量变永远无法导致质变  

　　自己推导  
　　- 掌握基础技术以及原理可以让自己推导答案和趋势  

###### 有哪些基础技术
　　编程语言  
　　- 原理、编程范式、设计模式、代码设计、类库  

　　系统  
　　- 计算机原理、操作系统、网络协议、数据库  

　　中间件  
　　- 消息队列、缓存、网关、代理  

　　理论知识  
　　- 算法和数据结构、系统架构、分布式  

###### 如何识别新的技术
　　解决了什么样的问题  
　　- 任何技术的出现都是要解决已有问题的  
　　- 降低技术门槛、提供开发效率、提升稳定性  

　　提升了什么样的能力  
　　- 可以计算更为复杂的计算  
　　- 可以自动化更为复杂和更为困难的事  

　　会成为主流技术的特征  
　　- 有大公司做背书  
　　- 有杀手级应用  
　　- 有强大的社区  

##### 3. 找到方法，事半功倍

###### 主动学习 VS 被动学习
![hello](/ARTS2020/ARTS-23/1.png)
　　- 翻译一本书  
　　- 比较多种语言的实现  

###### 学习的一些观点
　　- 学习是为了找到方法：学习不是找答案，而是找到通往答案的方法  

　　- 学习是为了认识原理和本质：理解原理和本质可以一通百通  

　　- 学习是为了打开自己的认知：你不知道你不知道的东西  

　　- 学习是为了改善自己：思维方式、行动方式  

###### 学习的相关方法
　　- 挑选知识和信息源：第一手资料非常重要（英文非常重要）  

　　- 注意基础和原理：我可以忘了这个技术，但是我可以自己徒手打造出来  

　　- 使用知识图系统的学习：通过知识关联可以进行“顺腾摸瓜”  

　　- 举一反三：用不同的方法学同一个东西，学一个东西时把周边的也学了  

　　- 总结和归纳：形成框架，套路和方法论  

　　- 实践和坚持：实践也能把知识变成技能，坚持才能有沉淀  

###### 学习的一些技巧
　　如何阅读代码  
　　- 基础知识、文档、代码结构  
　　- 模块、接口、关键业务路径  
　　- 代码逻辑、运行时调试  

　　如何面对枯燥和硬核的知识  
　　- 找到应用场景和牛人  
　　- 补充基础知识  
　　- 咬牙使劲啃  

　　其它小技巧  
　　- 不要记忆  
　　- 把信息压缩  
　　- 经常犯错  
　　- 写blog  
　　- 经常犯错  
　　- 它山之石可以攻玉  

###  <font color=indigo>Share: boost.io_state_savers库下的备忘录模式</font>
　　备忘录模式：在不违背封装原则的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便之后恢复对象为先前的状态。  

#### **【boost.io_state_savers库简介】**
　　saver类捕获一个对象的当前状态，并保存这个状态，在析构的时候便恢复对象先前的状态。  

　　我们来看下saver类的定义：  
```c++
class saver {
public:
    typedef std::ios_base state_type;
    typedef implementation_defined aspect_type;

    explicit saver(state_type& s);
    saver(state_type& s, const aspect_type& new_value);
    ~saver();

    void restore();
};
```
　　state_type是标准输入输出流的父类ios_base。用户通常会传递一个具体流对象给state_type参数。构造函数接受一个流对象，并保存这个流对象的引用和该对象的当前属性值。析构函数则会使用保存的属性值来恢复流对象。  

#### **【boost.io_state_savers库使用】**
　　第一个是不使用saver的例子：  
```c++
#include <iostream>

void print_hex(std::ostream& os, char byte) { os << std::hex << static_cast<unsigned>(byte) << std::endl; }

int main() {
    auto a = 'a';
    auto b = 'b';
    print_hex(std::cout, a);
    std::cout << static_cast<unsigned>(b) << std::endl;
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// 61
// 62
```
　　从结果中得知，在print_hex()函数中标准输出流cout状态被改变之后，并没有恢复。字符b仍然是16进制输出的。显然和我们预期的结果不吻合。  

　　接着我们来看使用saver的例子：  
```c++
#include <boost/io/ios_state.hpp>
#include <iostream>

void print_hex(std::ostream& os, char byte) {
    boost::io::ios_flags_saver ifs(os);
    os << std::hex << static_cast<unsigned>(byte) << std::endl;
}

int main() {
    auto a = 'a';
    auto b = 'b';
    print_hex(std::cout, a);
    std::cout << static_cast<unsigned>(b) << std::endl;
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 61
// 98
```
　　因为在ifs对象析构的时候，会调用restore()函数恢复之前的状态。因此字符b是以10进制输出的。  