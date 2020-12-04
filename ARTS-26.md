---
layout: post
title: ARST(二十六)
date: 2020-10-12 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=purple>Algorithm</font>

#### **【LeetCode:206. Reverse Linked List】**

##### 　　Description:
　　Reverse a singly linked list.  

##### 　　Example:
```sh
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/206.ReverseLinkedList/ReverseLinkedList.cpp  

###  <font color=purple>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
Semi-structured | adj. 半结构化的
workload | n. 工作量
ingest | vt. 摄取；咽下；吸收；接待
Leaderboard | n. 排行榜；通栏广告

#### **【Comments and Conclusions】**

##### SQL or NoSQL
　　Reasons for SQL:  
　　- Structured data  
　　- Strict schema  
　　- Relational data  
　　- Need for complex joins  
　　- Transactions  
　　- Clear patterns for scaling  
　　- More established: developers, community, code, tools, etc  
　　- Lookups by index are very fast  

　　Reasons for NoSQL:  
　　- Semi-structured data  
　　- Dynamic or flexible schema  
　　- Non-relational data  
　　- No need for complex joins  
　　- Store many TB (or PB) of data  
　　- Very data intensive workload  
　　- Very high throughput for IOPS  

　　Sample data well-suited for NoSQL:  
　　- Rapid ingest of clickstream and log data  
　　- Leaderboard or scoring data  
　　- Temporary data, such as a shopping cart  
　　- Frequently accessed ('hot') tables  
　　- Metadata/lookup tables  
　
###  <font color=purple>tips: 别闷头死学，咱得先看看企业需要哪些技术</font>
https://www.bilibili.com/video/BV1i7411L7Uj  

##### 中台较精准的定义
　　中台是为了体现IT技术、IT系统、IT部门的业务价值而诞生的概念  
![hello](/ARTS-26/1.jpg)

##### 技术人员需要构建个人的中台应对快速变化
　　公司创业方向（搜索引擎、容器、轻舟微服务）  
　　- 一个我擅长但是市场规模小，一个我不太擅长但是市场规模大？  
　　- 应该从市场规模、需求出发，再看公司的优势和竞争力，然后是方案。  

　　系统开发模式（DDD）  
　　- 从后台数据库出发，还是从业务领域出发？  
　　- 应该从业务流程、领域出发，数据库暂且位置不对，只是一时的痛，不是永远的痛。  

　　个人技术路线  
　　- 从市场需求出发，还是从个人技能出发？  
　　- 先看技术前景，再看如何打造个人竞争力，最后才是术。  
![hello](/ARTS-26/2.jpg)

##### 以业务为核心的技术体系建设总图
![hello](/ARTS-26/3.jpg)

##### 技术体系建设所涉及的架构和流程
![hello](/ARTS-26/4.jpg)

##### 以业务为核心的技术体系的演进过程
![hello](/ARTS-26/5.jpg)

##### 个人的技术体系建设总图
　　- 技术前景————看市场需求  
　　- 打造个人竞争力————参与核心项目  
　　- 积淀中台能力————有意识的构建可复用能力  

##### 第一：分布式文件系统（作者）
　　- 技术前景————看市场需求  
　　　　- 分布式存储崛起，S3 2006年，错过  

　　- 打造个人竞争力————参与核心项目  
　　　　- EMC分布式文件系统项目  

　　- 积淀中台能力————有意识的构建可复用能力  
　　　　- Hadoop-HDFS，Linux内核————《趣谈Linux操作系统》  

##### 第二：搜索引擎/大数据（作者）
　　- 技术前景————看市场需求  
　　　　- Google退出，Baidu崛起，国产搜索  

　　- 打造个人竞争力————参与核心项目  
　　　　- EMC文档管理系统业界第一，替代FAST，基于Lucene做搜索  
　　　　- 参与创业金融行业搜索引擎，深入Lucene，Hadoop-MR  
　　　　- 参与证券行业业务开发，接触SOA  

　　- 积淀中台能力————有意识的构建可复用能力  
　　　　- 大数据系列文章（公众号）  
　　　　- Hadoop-MR > 分布式计算调度算法 > Spark > Yarn > Mesos > 容器平台  
　　　　- Lucene搜索引擎————《Lucene应用开发解密》《Lucene原理与代码分析》 > Sensei > ElasticSearch  
　　　　- 接触SOA > 微服务  
　　　　- 接触分类聚类算法 > 机器学习/人工智能  

##### 第三：云计算OpenStack（作者）
　　- 技术前景————看市场需求  
　　　　- 云计算兴起，当时没有人直到云计算的确切定义，虚拟机和Hadoop哪个是云计算  
　　　　- 从Hadoop转向OpenStack  

　　- 打造个人竞争力————参与核心项目  
　　　　- HP，OpenStack业内贡献第一名  
　　　　- 华为，国内私有云第一  

　　- 积淀中台能力————有意识的构建可复用能力  
　　　　- OpenStack系列文章（公众号）  
　　　　- 数据中心，物理网络————《趣谈网络协议》  
　　　　- 虚拟网络Openvswitch————《趣谈网络协议》  
　　　　- 虚拟化————《趣谈网络协议》  
　　　　- 私有云平台建设全景视角（HP，华为）  

##### 第四：Docker容器平台（作者）
　　- 技术前景————看市场需求  
　　　　- Docker兴起，参与创业公司，公司要不要转向Docker  

　　- 打造个人竞争力————参与核心项目  
　　　　- 参与创业公司，会的是OpenStack，但是方向在Docker  
　　　　- 网易：大公司中较早上Docker和Kubernetes  

　　- 积淀中台能力————有意识的构建可复用能力  
　　　　- 容器平台系列文章（公众号）  
　　　　- 开始靠近应用层，微服务架构  

##### 第五：微服务（作者）
　　- 技术前景————看市场需求  
　　　　- 基础设施的演进大大超越了行业应用的演进  

　　- 打造个人竞争力————参与核心项目  
　　　　- 网易支撑考拉，云音乐，严选  

　　- 积淀中台能力————有意识的构建可复用能力  
　　　　- 微服务相关架构  
![hello](/ARTS-26/6.jpg)

![hello](/ARTS-26/7.jpg)

![hello](/ARTS-26/8.jpg)

##### 第六：业务层（作者）
![hello](/ARTS-26/9.jpg)

![hello](/ARTS-26/10.jpg)

##### 个人的技术体系建设总图
![hello](/ARTS-26/11.jpg)

###  <font color=purple>Share: 深入剖析单例模式（一）</font>

#### **【什么是单例模式？】**
　　《设计模式》一书中是这样定义的：保证一个类仅有一个实例，并提供一个访问它的全局访问点。  

　　单例模式非常好理解，就是一个类只能创建一个实例，客户仅能访问这个类的唯一实例。  

　　下面是单例模式的类图：  
![hello](/ARTS-26/12.jpg)

　　Singleton类中定义了一个Instance操作，供客户访问它的唯一实例uniqueInstance。  　

#### **【单例模式的应用场景】**
　　单例模式的应用场景非常简单。凡是需要保证设计类仅有一个实例供外界访问的原则，都可以用单例模式。  

　　比如配置信息类、ID生成器等等。  

#### **【单例模式的简单实现】**
　　下面我们来看下一个单例模式的c++实现：  
```c++
#include <iostream>
#include <memory>

class Singleton {
public:
    static std::shared_ptr<Singleton> Instance();
    ~Singleton() { std::cout << "~Singleton()" << std::endl; }

protected:
    Singleton() { std::cout << "Singleton()" << std::endl; }

private:
    static std::shared_ptr<Singleton> instance_;
};

std::shared_ptr<Singleton> Singleton::instance_;

std::shared_ptr<Singleton> Singleton::Instance() {
    if (instance_.get() == nullptr) {
        instance_ = std::shared_ptr<Singleton>(new Singleton());
    }
    return instance_;
}

int main() {
    Singleton::Instance();
    Singleton::Instance();
    Singleton::Instance();
    return 0;
}
// g++ -std=c++11 -Wall test.cpp -o test
// result:
// Singleton()
// ~Singleton()
```
　　在这个例子中，客户端通过Instance()成员函数访问Singleton类的唯一实例。尽管Singleton::Instance()调用了三次，但是Singleton类的构造函数只调用了一次。  

　　在Instance()成员函数中我们可以看出，它使用的是Lazy初始化（JAVA懒加载），只有第一次被访问时实例才会被创建出来。  

　　我们可以看出，这个单例模式是线程不安全的。要保证线程安全性，必须在Instance()的开始和结束时加锁和释放锁。这种情况下，如果频繁地获取实例，那么频繁地加锁、释放锁及并发度低等问题势必会导致性能瓶颈。  

　　后续我们会继续讨论其他几种单例模式的实现方式。  
