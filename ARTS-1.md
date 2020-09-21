---
layout: post
title: ARST(一)
date: 2020-04-20 9:30:00
tags: 
	- growing
	- ARST
---

### 什么是ARTS？
![hello](1.jpg)
　　来自耗子叔的建议：  
　　完成一个 ARTS 的时间不要超过 5 个小时，尽题控制在 2-3 个小时以内，少了你投入不够，多了难以坚持。  
　　这 2-3 个小时的时间分配是，算法题 30-60 分钟，英文文章 30 分钟，Tip 回想一下本周工作中学到的一个小技巧（10 分钟），Share思考一个技术观点、社会热点、一个产品或是一个困惑（这个时间应该放在日常），然后花 30-60 分钟写下来。  

###  <font color=red>Algorithm</font>

#### **【LeetCode:15. 3Sum】**

##### 　　Description:
　　Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

##### 　　Note:
　　The solution set must not contain duplicate triplets.

##### 　　Example:
```sh
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/015.3Sum/3Sum.cpp

###  <font color=red>Review</font>

#### **【Article】**
http://www.aosabook.org/en/distsys.html (1.1. Principles of Web Distributed Systems Design)

#### **【Words】**

English | Chinese
-|-
emerged | vt.出现；浮现，暴露
primitive | adj.原始的，远古的；简单的，粗糙的
tradeoff | n.权衡；折衷；（公平）交易
uptime | n.（计算机等的）正常运行时间
retail | adj. 零售的
resilient | adj. 弹回的，有弹力的；能复原的；可迅速恢复的
graceful | adj. 优雅的；优美的
degradation | n. 退化；降格，降级；堕落
correlate | vt. 使有相互关系；互相有关系
retention | n. 保留；扣留，滞留
optimize | vt. 使最优化，使完善
retrieval | n. 检索；恢复；取回；拯救
equate to | vi. 等同
facet | n. 面；方面；小平面
odd | n. 奇数；怪人；奇特的事物

#### **【Comments and Conclusions】**
　　Here're several key principles that influence the design of large-scale web systems: Availability, Performance, Reliability, Scalability, Manageability and cost.  
　　When designing any sort of web application it it important to consider these key principles, even if it is to acknowledge that a design may sacrifice one or more of them.  

###  <font color=red>tips：Linux命令：压缩和解压</font>

#### **【zip】**
　　.zip结尾的文件调用zip程序  
　　压缩：  
```sh
zip -r test.zip /home/test
```
　　解压：  
```sh
unzip test.zip
```

#### **【tar】**
　　.tar结尾的文件调用tar程序  
　　压缩：  
```sh
tar cvf test.tar /home/test
```
　　解压：  
```sh
tar xvf test.tar
```

#### **【tar.gz】**
　　.tar.gz结尾的文件调用tar程序，然后通过使用-z参数来调用gzip程序  
　　压缩：  
```sh
tar zcvf test.tar.gz /home/test
```
　　解压：  
```sh
tar zxvf test.tar.gz
```

#### **【tar.bz2】**
　　.tar.bz2结尾的文件调用tar程序，然后通过使用-j参数来调用bzip2程序  
　　压缩：  
```sh
tar jcvf test.tar.bz2 /home/test
```
　　解压：  
```sh
tar jxvf test.tar.bz2
```

###  <font color=red>Share：如何用 C 语言模拟面向对象四大特性？</font>

#### **【封装性】**
　　C语言没有访问权限控制，程序员只能通过结构体模拟类，将结构体的指针作为参数传入接口函数中。用户通过接口函数来操作结构体，而不是直接操作结构体中的数据。  
　　下面给一个例子：  
```c++
#include <stdio.h>
#include <string.h>

struct Player{
    int id;
    char name[20];
};

void InitPlayer(Player* player, int id, char* name, size_t nameLen){
    player->id = id;
    strncpy(player->name,name,nameLen+1);
}

int getPlayerId(Player* player){
    return player->id;
}

char* getPlayerName(Player* player){
    return player->name;
}

int main(){
    Player player;
    InitPlayer(&player,24,"Kobe",4);
    printf("id: %d\n",getPlayerId(&player));
    printf("name: %s\n",getPlayerName(&player));
}
```

#### **【抽象性】**
　　面向对象语言通过接口或者抽象类来实现面向对象的抽象性。  
　　C语言通过函数包裹具体的实现逻辑，提供函数名、文档给用户使用，这本身也是一种抽象。  

#### **【继承性】**
　　C语言通过结构体组合的方式来模拟面向对象的继承性，达到代码复用的目的。  

#### **【多态性】**
　　C语言利用函数指针来模拟面向对象的多态性。  
　　例子如下：  
```c++
#include <stdio.h>

void loggerRecord(){
    printf("I write a log into file.\n");
}

void DBRecord(){
    printf("I insert data into db.\n");
}

typedef void (*Record)();

void test(Record record){
    record();
}

int main(){
    test(loggerRecord);
    test(DBRecord);
}
```