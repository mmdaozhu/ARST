---
layout: post
title: ARST(二十九)
date: 2020-11-02 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=red>Algorithm</font>

#### **【LeetCode:236. Lowest Common Ancestor of a Binary Tree】**

##### 　　Description:
　　Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.  

　　According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”  

##### 　　Example 1:
![hello](/ARTS2020/ARTS-29/1.png)
```sh
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

##### 　　Example 2:
![hello](/ARTS2020/ARTS-29/2.png)
```sh
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

##### 　　Example 3:
```sh
Input: root = [1,2], p = 1, q = 2
Output: 1
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/236.LowestCommonAncestorOfABinaryTree/LowestCommonAncestorOfABinaryTree.cpp  

###  <font color=red>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
periodic | adj. 周期的；定期的
aggregation | n. 聚合，聚集；聚集体，集合体

#### **【Comments and Conclusions】**

##### Asynchronous
　　Asynchronous workflows help reduce request times for expensive operations that would otherwise be performed in-line. They can also help by doing time-consuming work in advance, such as periodic aggregation of data.  

##### Message queues
　　Message queues receive, hold, and deliver messages. If an operation is too slow to perform inline, you can use a message queue with the following workflow:  

　　- An application publishes a job to the queue, then notifies the user of job status  
　　- A worker picks up the job from the queue, processes it, then signals the job is complete  

　　The user is not blocked and the job is processed in the background. During this time, the client might optionally do a small amount of processing to make it seem like the task has completed. For example, if posting a tweet, the tweet could be instantly posted to your timeline, but it could take some time before your tweet is actually delivered to all of your followers.  

　　Redis is useful as a simple message broker but messages can be lost.  

　　RabbitMQ is popular but requires you to adapt to the 'AMQP' protocol and manage your own nodes.  

　　Amazon SQS is hosted but can have high latency and has the possibility of messages being delivered twice.  

##### Task queues
　　Tasks queues receive tasks and their related data, runs them, then delivers their results. They can support scheduling and can be used to run computationally-intensive jobs in the background.  

　　Celery has support for scheduling and primarily has python support.  

##### Back pressure
　　If queues start to grow significantly, the queue size can become larger than memory, resulting in cache misses, disk reads, and even slower performance. Back pressure can help by limiting the queue size, thereby maintaining a high throughput rate and good response times for jobs already in the queue. Once the queue fills up, clients get a server busy or HTTP 503 status code to try again later. Clients can retry the request at a later time, perhaps with exponential backoff.  

##### Disadvantage(s): asynchronism
　　Use cases such as inexpensive calculations and realtime workflows might be better suited for synchronous operations, as introducing queues can add delays and complexity.  


###  <font color=red>tips: 程序员不会写文作？</font>
https://www.bilibili.com/video/BV1f7411T7hQ  

##### 以说代写
　　用讯飞听见（软件），将想写的说下来，在基础上进行修改  

##### 对象感，问问题
　　看下风格感觉（书），将问的问题串联成一个故事  

##### 多读书，多思考

###  <font color=red>Share: 浅谈抽象工厂模式</font>

#### **【什么是抽象工厂？】**
　　《设计模式》一书中是这样定义的：提供一个接口以创建一系列相关或相互依赖的对象，而无须指定它们具体的类。  

　　我们来看下工厂方法的类图：  
![hello](/ARTS2020/3.jpg)

　　AbstractFactory提供了一个接口来创建相关的AbstractProductA和AbstractProductB对象。Client通过不同的ConcreteFactory类的接口，创建不同的ProductA和ProductB对象。  

　　抽象工厂和工厂方法不同之处在于：抽象工厂是创建一系列相互依赖的类的对象，而工厂方法是创建一个类的不同对象。  

#### **【抽象工厂的优缺点】**

##### 优点
　　1. 基于接口而非实现编程：Client类使用AbstractFactory、AbstractProductA、AbstractProductB等抽象类提供的接口，使和类的实现分类。这样就方便客户代码替换产品系列，客户代码仅仅需要修改具体的ConcreteFactory类，就能够替换另一系列的产品。  

　　2. 有利于产品的一致性：AbstractFactory提供的接口中定义了一系列相互依赖的对象，这也规定了客户只能使用这一系列相关依赖的对象。  

##### 缺点
　　1. 难以扩展新种类的产品：有利于产品的一致性也有其缺点，如果想在AbstractFactory类中新增一个产品类，其子类也必须新增这样的一个产品类。  

#### **【抽象工厂的应用场景】**
　　一个系统需要由多个产品系列中的一个来配置，并且要对立于产品的创建、组合和表示。  

#### **【抽象工厂的实现方法】**
　　迷宫游戏使用抽象工厂模式创建迷宫，抽象工厂中包含创建迷宫、房间、门等一系列对象。  

　　具体看如下代码：  
```c++
#include <iostream>
#include <memory>
#include <vector>

/*****************************************Room***************************************/
class Room {
public:
    Room(int id) {
        std::cout << "Room()" << std::endl;
        std::cout << "Room id: " << id << std::endl;
    }
    virtual ~Room() {}
};

class EnchantedRoom : public Room {
public:
    EnchantedRoom(int id) : Room(id) {
        std::cout << "EnchantedRoom()" << std::endl;
        std::cout << "EnchantedRoom id: " << id << std::endl;
    }
};

class BombedRoom : public Room {
public:
    BombedRoom(int id) : Room(id) {
        std::cout << "BombedRoom()" << std::endl;
        std::cout << "BombedRoom id: " << id << std::endl;
    }
};

/*****************************************Maze***************************************/
class Maze {
public:
    Maze() { std::cout << "Maze()" << std::endl; }
    virtual ~Maze() {}
    void AddRoom(const std::shared_ptr<Room>& room) { rooms_.emplace_back(room); }

private:
    std::vector<std::shared_ptr<Room>> rooms_;
};

class EnchantedMaze : public Maze {
public:
    EnchantedMaze() { std::cout << "EnchantedMaze()" << std::endl; }
    void PrintEnchanted() { std::cout << "PrintEnchanted" << std::endl; }
};

class BombedMaze : public Maze {
public:
    BombedMaze() { std::cout << "BombedMaze()" << std::endl; }
    void PrintBombed() { std::cout << "PrintBombed" << std::endl; }
};

/*****************************************MazeFactory***************************************/
class MazeFactory {
public:
    ~MazeFactory() {}
    virtual std::shared_ptr<Maze> MakeMaze() { return std::make_shared<Maze>(); }

    virtual std::shared_ptr<Room> MakeRoom(int id) { return std::make_shared<Room>(id); }
};

class EnchantedMazeFactory : public MazeFactory {
public:
    virtual std::shared_ptr<Maze> MakeMaze() { return std::make_shared<EnchantedMaze>(); }

    virtual std::shared_ptr<Room> MakeRoom(int id) { return std::make_shared<EnchantedRoom>(id); }
};

class BombedFactory : public MazeFactory {
public:
    virtual std::shared_ptr<Maze> MakeMaze() { return std::make_shared<BombedMaze>(); }

    virtual std::shared_ptr<Room> MakeRoom(int id) { return std::make_shared<BombedRoom>(id); }
};

class MazeGame {
public:
    std::shared_ptr<Maze> CreateMaze(MazeFactory& factory) {
        std::shared_ptr<Maze> maze = factory.MakeMaze();
        std::shared_ptr<Room> room1 = factory.MakeRoom(1);
        std::shared_ptr<Room> room2 = factory.MakeRoom(2);
        maze->AddRoom(room1);
        maze->AddRoom(room2);

        std::shared_ptr<EnchantedMaze> enchantedmaze = std::dynamic_pointer_cast<EnchantedMaze>(maze);
        if (enchantedmaze) {
            enchantedmaze->PrintEnchanted();
        }
        std::shared_ptr<BombedMaze> bombedmaze = std::dynamic_pointer_cast<BombedMaze>(maze);
        if (bombedmaze) {
            bombedmaze->PrintBombed();
        }
        return maze;
    }
};

int main() {
    {
        MazeGame maze_game;
        EnchantedMazeFactory enchantedmaze_factory;
        std::shared_ptr<Maze> maze = maze_game.CreateMaze(enchantedmaze_factory);
    }
    std::cout << std::endl;
    {
        MazeGame maze_game;
        BombedFactory bombedmaze_factory;
        std::shared_ptr<Maze> maze = maze_game.CreateMaze(bombedmaze_factory);
    }
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// Maze()
// EnchantedMaze()
// Room()
// Room id: 1
// EnchantedRoom()
// EnchantedRoom id: 1
// Room()
// Room id: 2
// EnchantedRoom()
// EnchantedRoom id: 2
// PrintEnchanted

// Maze()
// BombedMaze()
// Room()
// Room id: 1
// BombedRoom()
// BombedRoom id: 1
// Room()
// Room id: 2
// BombedRoom()
// BombedRoom id: 2
// PrintBombed
```

　　MazeGame类中使用CreateMaze成员函数来创建迷宫，将MazeFactory作为一个参数。这样客户就可以通过MazeFactory的子类EnchantedMazeFactory和BombedFactory来创建不同的迷宫。  

　　MazeFactory类以及其子类使用的是工厂模式。只不过它们创建不是同一类对象，而是相互联系的一系列类的对象。  

　　在CreateMaze成员函数中，创建了一个迷宫和两个门，并将门放入到迷宫中。值得注意的时候，如果客户需要进行特定子类的相关操作，那么必须进行向下类型转换，尽管这种转换是不安全的。  