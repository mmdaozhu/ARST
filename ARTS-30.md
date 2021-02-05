---
layout: post
title: ARST(三十)
date: 2020-11-09 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=red>Algorithm</font>

#### **【LeetCode:50. Pow(x, n)】**

##### 　　Description:
　　Implement pow(x, n), which calculates x raised to the power n (i.e. xn)  

##### 　　Example 1:
```sh
Input: x = 2.00000, n = 10
Output: 1024.00000
```

##### 　　Example 2:
```sh
Input: x = 2.10000, n = 3
Output: 9.26100
```

##### 　　Example 3:
```sh
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

##### 　　C++ Solution 1:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/050.PowXN/PowXN1.cpp  

##### 　　C++ Solution 2:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/050.PowXN/PowXN2.cpp  

###  <font color=red>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
Idempotent | adj. 幂等的
congestion | n. 拥挤；拥塞；淤血
intact | adj. 完整的；原封不动的；未受损伤的

#### **【Comments and Conclusions】**

##### Communication

##### Hypertext transfer protocol (HTTP)
　　HTTP is a method for encoding and transporting data between a client and a server. It is a request/response protocol: clients issue requests and servers issue responses with relevant content and completion status info about the request. HTTP is self-contained, allowing requests and responses to flow through many intermediate routers and servers that perform load balancing, caching, encryption, and compression.  

　　A basic HTTP request consists of a verb (method) and a resource (endpoint). Below are common HTTP verbs:  

Verb | Description | Idempotent* | Safe | Cacheable
-|-|-|-|-
GET | Reads a resource | Yes | Yes | Yes
POST | Creates a resource or trigger a process that handles data | No | No | Yes if response contains freshness info
PUT | Creates or replace a resource | Yes | No | No
PATCH | Partially updates a resource | No | N | Yes if response contains freshness info
DELETE | Deletes a resource | Yes | No | No

　　HTTP is an application layer protocol relying on lower-level protocols such as TCP and UDP.  

##### Transmission control protocol (TCP)
　　TCP is a connection-oriented protocol over an IP network. Connection is established and terminated using a handshake. All packets sent are guaranteed to reach the destination in the original order and without corruption through:  

　　- Sequence numbers and checksum fields for each packet  
　　- Acknowledgement packets and automatic retransmission  

　　If the sender does not receive a correct response, it will resend the packets. If there are multiple timeouts, the connection is dropped. TCP also implements flow control and congestion control. These guarantees cause delays and generally result in less efficient transmission than UDP.  

　　To ensure high throughput, web servers can keep a large number of TCP connections open, resulting in high memory usage. It can be expensive to have a large number of open connections between web server threads and say, a memcached server. Connection pooling can help in addition to switching to UDP where applicable.  

　　TCP is useful for applications that require high reliability but are less time critical. Some examples include web servers, database info, SMTP, FTP, and SSH.  

　　Use TCP over UDP when:  

　　- You need all of the data to arrive intact  
　　- You want to automatically make a best estimate use of the network throughput  
　
###  <font color=red>tips: 如何不让简历投放后石沉大海</font>
https://www.bilibili.com/video/BV1QE411F7gC  

##### 应聘的公司为什么在招人？
　　1. 填补空缺的职位  
　　2. 新增职位  

##### 入职前明确岗位的性质，搞清楚在为谁卖命
　　1. 实习生  
　　2. 正式员工  

##### 一秒钟了解投简历的规则
　　1. 邮件格式要正确  
　　2. 标题要明确  
　　3. 邮件正文要简洁  

###  <font color=red>Share: 浅谈生成器模式</font>

#### **【什么是生成器模式？】**
　　《设计模式》一书中是这样定义的：将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。  

　　我们来看下生成器模式的类图：  
![hello](/ARTS-30/1.jpg)  

　　Director类使用Builder对象提供的接口，使得其Construct算法与Builder接口提供的对象的构建与装配分离。  

　　Builder对象完成Product的创建与装配后返回给Director类。  

　　接着我们看下生成器模式的交互图：  
![hello](/ARTS-30/2.jpg)  

　　过程如下：  
　　1. aClient创建Director对象，并使用想要的Builder对象进行配置。  
　　2. 生成器处理导向器的请求，并将部件增加到产品中。  
　　3. aClient从生成器中检索产品。  

#### **【生成器模式的优点】**
　　1. 封装一个产品的内部表示：Builder对象提供给Director对象一个构造产品的抽象接口。该接口隐藏了这个产品的表示和内部结构，同时也隐藏了该产品是如何装配的。  

　　2. 将构造代码和表示代码分离：Builder模式通过封装一个复杂对象的创建和表示方式，提供了对象的模块性。  

#### **【生成器模式的应用场景】**
　　当创建复杂对象的算法与该对象的组成部分以及它们的装配方式独立，并且构造过程必须允许被构造的对象有不同的表示。  

#### **【生成器的实现方法】**
　　迷宫游戏使用生成器模式创建迷宫，并装配迷宫、房间、门等一系列对象。  

　　具体看如下代码：  
```c++
#include <iostream>
#include <memory>
#include <vector>

class Room {
public:
    Room(int id) : id_(id) {
        std::cout << "Room()" << std::endl;
        std::cout << "Room id: " << id << std::endl;
    }
    ~Room() {}
    int GetId() const { return id_; }

private:
    int id_;
};

class Door {
public:
    Door(const std::shared_ptr<Room>& room1, const std::shared_ptr<Room>& room2) {
        std::cout << "Door()" << std::endl;
        std::cout << "room1 id: " << room1->GetId() << std::endl;
        std::cout << "room2 id: " << room2->GetId() << std::endl;
    }
};

class Maze {
public:
    Maze() { std::cout << "Maze()" << std::endl; }
    ~Maze() {}
    void AddRoom(const std::shared_ptr<Room>& room) { rooms_.emplace_back(room); }
    void AddDoor(const std::shared_ptr<Door>& door) { doors_.emplace_back(door); }
    std::shared_ptr<Room> RoomNo(int n) {
        for (const auto& room : rooms_) {
            if (room->GetId() == n) {
                return room;
            }
        }
        return std::shared_ptr<Room>();
    }

private:
    std::vector<std::shared_ptr<Room>> rooms_;
    std::vector<std::shared_ptr<Door>> doors_;
};

class MazeBuilder {
public:
    virtual ~MazeBuilder() {}
    virtual void BuildMaze() {}
    virtual void BuildRoom(int n) {}
    virtual void BuildDoor(int room1, int room2) {}
    virtual std::shared_ptr<Maze> GetMaze() { return std::shared_ptr<Maze>(); }

protected:
    MazeBuilder() {}
};

class StandardMazeBuilder : public MazeBuilder {
public:
    StandardMazeBuilder() {}
    ~StandardMazeBuilder() {}
    virtual void BuildMaze() { current_maze_ = std::make_shared<Maze>(); }
    virtual void BuildRoom(int n) {
        if (!current_maze_->RoomNo(n)) {
            std::shared_ptr<Room> room = std::make_shared<Room>(n);
            current_maze_->AddRoom(room);
        }
    }
    virtual void BuildDoor(int room1, int room2) {
        std::shared_ptr<Room> r1 = current_maze_->RoomNo(room1);
        std::shared_ptr<Room> r2 = current_maze_->RoomNo(room2);
        std::shared_ptr<Door> door = std::make_shared<Door>(r1, r2);
        current_maze_->AddDoor(door);
    }

    virtual std::shared_ptr<Maze> GetMaze() { return current_maze_; }

private:
    std::shared_ptr<Maze> current_maze_;
};

class MazeGame {
public:
    std::shared_ptr<Maze> CreateMaze(MazeBuilder& builder) {
        builder.BuildMaze();
        builder.BuildRoom(1);
        builder.BuildRoom(2);
        builder.BuildDoor(1, 2);
        return builder.GetMaze();
    }
};

int main() {
    StandardMazeBuilder builder;
    MazeGame game;
    game.CreateMaze(builder);
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// Maze()
// Room()
// Room id: 1
// Room()
// Room id: 2
// Door()
// room1 id: 1
// room2 id: 2
```

　　MazeGame类中使用CreateMaze成员函数来创建迷宫，并使用StandardMazeBuilder对象进行配置。在函数中，StandardMazeBuilder对象处理MazeGame的请求，来创建Maze、Room、Door等对象，并采用合适的方式进行装配，最终返回一个Maze对象给MazeGame。  

　　MazeBuilder封装了创建产品和装配的过程，对MazeGame类是隐藏的。代码采用生成器模式对类的关系进行了有效的解耦，提高了代码的封装性和复用性。  



　　
