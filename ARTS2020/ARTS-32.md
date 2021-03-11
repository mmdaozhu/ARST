---
layout: post
title: ARST(三十二)
date: 2020-11-23 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=red>Algorithm</font>

#### **【LeetCode:22. Generate Parentheses】**

##### 　　Description:
　　Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.  

##### 　　Example 1:
```sh
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

##### 　　Example 2:
```sh
Input: n = 1
Output: ["()"]
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/022.GenerateParentheses/GenerateParentheses.cpp  

###  <font color=red>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
reinvent | vt. 重新使用；彻底改造；重复发明
hierarchy | n. 层级；等级制度

#### **【Comments and Conclusions】**

##### Representational state transfer (REST)
　　REST is an architectural style enforcing a client/server model where the client acts on a set of resources managed by the server. The server provides a representation of resources and actions that can either manipulate or get a new representation of resources. All communication must be stateless and cacheable.  

　　There are four qualities of a RESTful interface:  
　　- Identify resources (URI in HTTP) - use the same URI regardless of any operation.  
　　- Change with representations (Verbs in HTTP) - use verbs, headers, and body.  
　　- Self-descriptive error message (status response in HTTP) - Use status codes, don't reinvent the wheel.  
　　- HATEOAS (HTML interface for HTTP) - your web service should be fully accessible in a browser.  

　　REST is focused on exposing data. It minimizes the coupling between client/server and is often used for public HTTP APIs. REST uses a more generic and uniform method of exposing resources through URIs, representation through headers, and actions through verbs such as GET, POST, PUT, DELETE, and PATCH. Being stateless, REST is great for horizontal scaling and partitioning.  

　　Disadvantage(s): REST  
　　- With REST being focused on exposing data, it might not be a good fit if resources are not naturally organized or accessed in a simple hierarchy. For example, returning all updated records from the past hour matching a particular set of events is not easily expressed as a path. With REST, it is likely to be implemented with a combination of URI path, query parameters, and possibly the request body.  
　　- REST typically relies on a few verbs (GET, POST, PUT, DELETE, and PATCH) which sometimes doesn't fit your use case. For example, moving expired documents to the archive folder might not cleanly fit within these verbs.  
　　- Fetching complicated resources with nested hierarchies requires multiple round trips between the client and server to render single views, e.g. fetching content of a blog entry and the comments on that entry. For mobile applications operating in variable network conditions, these multiple roundtrips are highly undesirable.  
　　- Over time, more fields might be added to an API response and older clients will receive all new data fields, even those that they do not need, as a result, it bloats the payload size and leads to larger latencies.  

##### RPC and REST calls comparison
| Operation | RPC | REST |
|---|---|---|
| Signup    | **POST** /signup | **POST** /persons |
| Resign    | **POST** /resign<br/>{<br/>"personid": "1234"<br/>} | **DELETE** /persons/1234 |
| Read a person | **GET** /readPerson?personid=1234 | **GET** /persons/1234 |
| Read a person’s items list | **GET** /readUsersItemsList?personid=1234 | **GET** /persons/1234/items |
| Add an item to a person’s items | **POST** /addItemToUsersItemsList<br/>{<br/>"personid": "1234";<br/>"itemid": "456"<br/>} | **POST** /persons/1234/items<br/>{<br/>"itemid": "456"<br/>} |
| Update an item    | **POST** /modifyItem<br/>{<br/>"itemid": "456";<br/>"key": "value"<br/>} | **PUT** /items/456<br/>{<br/>"key": "value"<br/>} |
| Delete an item | **POST** /removeItem<br/>{<br/>"itemid": "456"<br/>} | **DELETE** /items/456 |
　
　
###  <font color=red>tips: stack unwinding</font>
　　stack unwinding: When an exception is thrown and the control returns to its function, the c++ run time calls destructors of all automatic objects constructed since the beginning of the try block.  

　　Example:  
```c++
#include <stdio.h>

class Obj {
public:
    Obj() { puts("Obj()"); }
    ~Obj() { puts("~Obj()"); }
};

void foo(int n) {
    Obj obj;
    if (n == 42) throw "life, the universe and everything";
}

int main() {
    try {
        foo(41);
        foo(42);
    } catch (const char* s) {
        puts(s);
    }
}
// result:
// Obj()
// ~Obj()
// Obj()
// ~Obj()
// life, the universe and everything
```

###  <font color=red>Share: 高性能数据库集群：读写分离</font>

#### **【读写分离原理】**
　　<b>读写分离的基本原理是将数据库读写操作分散到不同的节点上，</b>下面是其基本架构图。  
![hello](/ARTS2020/ARTS-32/1.jpg)  

　　读写分离的基本实现是：  

　　- 数据库服务器搭建主从集群，一主一从、一主多从都可以。  
　　- 数据库主机负责读写操作，从机只负责读操作。  
　　- 数据库主机通过复制将数据同步到从机，每台数据库服务器都存储了所有的业务数据。  
　　- 业务服务器将写操作发给数据库主机，将读操作发给数据库从机。  

　　读写分离的实现逻辑并不复杂，但有两个细节点将引入设计复杂度：主从复制延迟和分配机制。  

#### **【设计复杂度：主从复制延迟】**
　　主从复制延迟会带来一个问题：如果业务服务器将数据写入到数据库主服务器后立刻（1 秒内）进行读取，此时读操作访问的是从机，主机还没有将数据复制过来，到从机读取数据是读不到最新数据的，业务上就可能出现问题。  

#### **【设计复杂度：分配机制】**
　　将读写操作区分开来，然后访问不同的数据库服务器，一般有两种方式：程序代码封装和中间件封装。  

##### 1. 程序代码封装
　　程序代码封装指在代码中抽象一个数据访问层（所以有的文章也称这种方式为“中间层封装”），实现读写操作分离和数据库服务器连接的管理。基本架构是：  
![hello](/ARTS2020/ARTS-32/2.jpg)  

##### 2. 中间件封装
　　中间件封装指的是独立一套系统出来，实现读写操作分离和数据库服务器连接的管理。中间件对业务服务器提供 SQL 兼容的协议，业务服务器无须自己进行读写分离。对于业务服务器来说，访问中间件和访问数据库没有区别，事实上在业务服务器看来，中间件就是一个数据库服务器。其基本架构是：  
![hello](/ARTS2020/ARTS-32/3.jpg)  

　　由于数据库中间件的复杂度要比程序代码封装高出一个数量级，一般情况下建议采用程序语言封装的方式，或者使用成熟的开源数据库中间件。如果是大公司，可以投入人力去实现数据库中间件，因为这个系统一旦做好，接入的业务系统越多，节省的程序开发投入就越多，价值也越大。  

　　目前的开源数据库中间件方案中，MySQL 官方先是提供了 MySQL Proxy，但 MySQL Proxy 一直没有正式 GA，现在 MySQL 官方推荐 MySQL Router。MySQL Router 的主要功能有读写分离、故障自动切换、负载均衡、连接池等，其基本架构如下：  
![hello](/ARTS2020/ARTS-32/4.jpg)  

#### **【脑图】**
![hello](/ARTS2020/ARTS-32/读写分离.png)  

#### **【思考题】**
　　数据库读写分离一般应用于什么场景？能支撑多大的业务规模？  

　　读写分离使用单机并发无法支持并且读的请求非常多的场景。如果服务器有性能问题，应该首先采取SQL优化、引入缓存等方式。  

