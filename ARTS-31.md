---
layout: post
title: ARST(三十一)
date: 2020-11-16 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=red>Algorithm</font>

#### **【LeetCode:102. Binary Tree Level Order Traversal】**

##### 　　Description:
　　Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).  

##### 　　Example:
```sh
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
   

return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]
```

##### 　　C++ Solution 1:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/102.BinaryTreeLevelOrderTraversal/BinaryTreeLevelOrderTraversal1.cpp  

##### 　　C++ Solution 2:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/102.BinaryTreeLevelOrderTraversal/BinaryTreeLevelOrderTraversal2.cpp  

###  <font color=red>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
stub | n. 存根；烟蒂；树桩；断株
Marshals | v. 排列；安排

#### **【Comments and Conclusions】**

##### Remote procedure call (RPC)
　　In an RPC, a client causes a procedure to execute on a different address space, usually a remote server. The procedure is coded as if it were a local procedure call, abstracting away the details of how to communicate with the server from the client program. Remote calls are usually slower and less reliable than local calls so it is helpful to distinguish RPC calls from local calls. Popular RPC frameworks include Protobuf, Thrift, and Avro.  

　　RPC is a request-response protocol:  

　　- Client program - Calls the client stub procedure. The parameters are pushed onto the stack like a local procedure call.  
　　- Client stub procedure - Marshals (packs) procedure id and arguments into a request message.  
　　- Client communication module - OS sends the message from the client to the server.  
　　- Server communication module - OS passes the incoming packets to the server stub procedure.  
　　- Server stub procedure - Unmarshalls the results, calls the server procedure matching the procedure id and passes the given arguments.  
　　- The server response repeats the steps above in reverse order.  

　　Disadvantage(s): RPC  
　　- RPC clients become tightly coupled to the service implementation.  
　　- A new API must be defined for every new operation or use case.  
　　- It can be difficult to debug RPC.  
　　- You might not be able to leverage existing technologies out of the box. For example, it might require additional effort to ensure RPC calls are properly cached on caching servers such as Squid.  
　
###  <font color=red>tips: SSL证书</font>

#### **【SSL Certificates】**
　　SSL 证书就是遵守 SSL协议，由受信任的数字证书颁发机构CA，在验证服务器身份后颁发，具有服务器身份验证和数据传输加密功能。  

　　一个有效、可信的 SSL 数字证书包括一个公共密钥和一个私用密钥。公共密钥用于加密信息，私用密钥用于解密信息。  

#### **【SSL Certificates 自签名证书请求信息】**
```c++
struct SelfSignedCert{
    std::string country;// Required
    std::string state_or_province;// Required
    std::string city_or_Locality; //Required
    std::string organization; //Required
    std::string common_name; //BMC Host Name Required
    std::string contact_person; //Optional
    std::string email; //Optional
    std::string organizational_unit; //Optional
    std::string surname; //Optional
    std::string given_name; //Optional
    std::string initials; //Optional
    std::string dn_qualifier; //Optional
}
```

#### **【CSR】**
　　CSR就是Certificate Signing Request证书请求文件。  

#### **【CSR请求信息】**
```c++
struct SelfSignedCert{
    std::string country;// Required
    std::string state_or_province;// Required
    std::string city_or_Locality; //Required
    std::string organization; //Required
    std::string common_name; //BMC Host Name Required
    std::string contact_person; //Optional
    std::string email; //Optional
    std::string organizational_unit; //Optional
    std::string surname; //Optional
    std::string given_name; //Optional
    std::string initials; //Optional
    std::string dn_qualifier; //Optional
    std::string challenge_password; //Optional
    std::string unstructured_name; //Optional
}
```

#### **【证书操作】**
　　1. 用户连接到你的Web站点，该Web站点受服务器证书所保护。  

　　2. 你的服务器进行响应，并自动传送你网站的数字证书给用户，用于鉴别你的网站。  

　　3. 用户的网页浏览器程序产生一把唯一的“会话钥匙码“，用以跟网站之间所有的通讯过程进行加密。  

　　4. 使用者的浏览器以网站的公钥对交谈钥匙码进行加密，以便只有让你的网站得以阅读此交谈钥匙码。  

#### **【如何申请】**
　　1. 制作CSR文件: 这个文件是由申请人制作，在制作的同时，系统会产生2个密钥，一个是公钥就是这个CSR文件，另外一个是私钥，存放在服务器上。  

　　2. CA认证: 将CSR提交给CA，CA一般有2种认证方式：1.域名认证，2.企业文档认证。  

　　3. 证书的安装： 在收到CA的证书后，可以将证书部署上服务器。  

###  <font color=red>Share: 浅谈原型模式</font>
　　《设计模式》一书中是这样定义原型模式：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。  

　　我们来看下原型模式的类图：  
![hello](/ARTS-31/1.jpg)  

　　　原型模式很简单，客户Client使用ProtoType类或其子类作为原型，获取一个通过原型复制自身而创建的对象。原型模式又分为浅拷贝和深拷贝。  

#### **【原型模式的优缺点】**

##### 优点
　　1. 动态的增加和删除产品：可以通过客户注册原型实例将一个具体的产品类加入系统，可以在运行时建立和删除原型。  

　　2. 减少子类的构造：不同于工厂方法，原型模式允许你复制一个原型就可以产生一个新的对象，不需要工厂类层次，大大减少了子类。  

##### 缺点
　　ProtoType的子类必须实现Clone操作。  

#### **【原型模式的应用场景】**
　　1. 当实例化的类是动态指定时。  

　　2. 当类的实例只能有几个不同状态组合中的一种时。  

#### **【原型模式的实现方法】**
　　迷宫游戏使用原型模式创建迷宫，房间、门等一系列对象。  

##### 浅拷贝：
　　我们直接上代码：  
```c++
#include <iostream>
#include <memory>
#include <vector>

class Room {
public:
    Room() : id_(0) {
        std::cout << "Prototype Room()" << std::endl;
        std::cout << "Room id: " << id_ << std::endl;
    }

    Room(int id) : id_(id) {
        std::cout << "Room()" << std::endl;
        std::cout << "Room id: " << id_ << std::endl;
    }

    Room(const Room& other) {
        std::cout << "Copy Room()" << std::endl;
        id_ = other.id_;
    }

    ~Room() { std::cout << "~Room()" << std::endl; }

    void Init(int id) { id_ = id; }

    virtual std::shared_ptr<Room> Clone() { return std::make_shared<Room>(*this); }

    int GetId() const { return id_; }

private:
    int id_;
};

class BombedRoom : public Room {
public:
    BombedRoom() : Room() { std::cout << "Prototype BombedRoom()" << std::endl; }

    BombedRoom(int id) : Room(id) { std::cout << "BombedRoom()" << std::endl; }

    BombedRoom(const BombedRoom& other) : Room(other) {
        std::cout << "Copy BombedRoom()" << std::endl;
        bombed_ = other.bombed_;
    }

    ~BombedRoom() { std::cout << "~BombedRoom()" << std::endl; }

    virtual std::shared_ptr<Room> Clone() { return std::make_shared<BombedRoom>(*this); }

private:
    bool bombed_;
};

class Maze {
public:
    Maze() { std::cout << "Maze()" << std::endl; }
    Maze(const Maze& other) {
        std::cout << "Copy Maze()" << std::endl;
        rooms_ = other.rooms_;
    }
    ~Maze() { std::cout << "~Maze()" << std::endl; }

    virtual std::shared_ptr<Maze> Clone() { return std::make_shared<Maze>(*this); }
    void AddRoom(const std::shared_ptr<Room>& room) {
        std::cout << "AddRoom() Room id: " << room->GetId() << std::endl;
        rooms_.emplace_back(room);
    }

private:
    std::vector<std::shared_ptr<Room>> rooms_;
};

class MazeFactory {
public:
    ~MazeFactory() {}
    virtual std::shared_ptr<Maze> MakeMaze() {}

    virtual std::shared_ptr<Room> MakeRoom(int id) {}
};

class MazePrototypeFactory : public MazeFactory {
public:
    MazePrototypeFactory(const std::shared_ptr<Maze>& maze, const std::shared_ptr<Room>& room)
        : prototype_maze_(maze), prototype_room_(room) {}

    virtual std::shared_ptr<Maze> MakeMaze() { return prototype_maze_->Clone(); }

    virtual std::shared_ptr<Room> MakeRoom(int id) {
        std::shared_ptr<Room> room = prototype_room_->Clone();
        room->Init(id);
        return room;
    }

private:
    std::shared_ptr<Maze> prototype_maze_;
    std::shared_ptr<Room> prototype_room_;
};

class MazeGame {
public:
    std::shared_ptr<Maze> CreateMaze(MazeFactory& factory) {
        std::shared_ptr<Maze> maze = factory.MakeMaze();
        std::shared_ptr<Room> room1 = factory.MakeRoom(1);
        std::shared_ptr<Room> room2 = factory.MakeRoom(2);
        maze->AddRoom(room1);
        maze->AddRoom(room2);
        return maze;
    }
};

int main() {
    std::shared_ptr<Maze> maze = std::make_shared<Maze>();
    maze->AddRoom(std::make_shared<Room>(3));

    MazePrototypeFactory factory(maze, std::make_shared<BombedRoom>());
    MazeGame game;
    game.CreateMaze(factory);
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// Maze()
// Room()
// Room id: 3
// AddRoom() Room id: 3
// Prototype Room()
// Room id: 0
// Prototype BombedRoom()
// Copy Maze()
// Copy Room()
// Copy BombedRoom()
// Copy Room()
// Copy BombedRoom()
// AddRoom() Room id: 1
// AddRoom() Room id: 2
// ~Maze()
// ~BombedRoom()
// ~Room()
// ~BombedRoom()
// ~Room()
// ~BombedRoom()
// ~Room()
// ~Maze()
// ~Room()
```

　　MazePrototypeFactory类使用Maze原型类和BombedRoom原型类来创建Maze，通过使用其Clone成员方法来复制自身。  

　　在Maze类中，我们看到Maze的拷贝构造函数中，成员变量rooms_是直接赋值指针的，也就是浅拷贝。可以通过程序输出看出，Room 3只构造了一次，析构了一次。  

##### 深拷贝：
　　接着看下深拷贝的实现方式：  
```c++
#include <iostream>
#include <memory>
#include <vector>

class Room {
public:
    Room() : id_(0) {
        std::cout << "Prototype Room()" << std::endl;
        std::cout << "Room id: " << id_ << std::endl;
    }

    Room(int id) : id_(id) {
        std::cout << "Room()" << std::endl;
        std::cout << "Room id: " << id_ << std::endl;
    }

    Room(const Room& other) {
        std::cout << "Copy Room()" << std::endl;
        id_ = other.id_;
    }

    ~Room() { std::cout << "~Room()" << std::endl; }

    void Init(int id) { id_ = id; }

    virtual std::shared_ptr<Room> Clone() { return std::make_shared<Room>(*this); }

    int GetId() const { return id_; }

private:
    int id_;
};

class BombedRoom : public Room {
public:
    BombedRoom() : Room() { std::cout << "Prototype BombedRoom()" << std::endl; }

    BombedRoom(int id) : Room(id) { std::cout << "BombedRoom()" << std::endl; }

    BombedRoom(const BombedRoom& other) : Room(other) {
        std::cout << "Copy BombedRoom()" << std::endl;
        bombed_ = other.bombed_;
    }

    ~BombedRoom() { std::cout << "~BombedRoom()" << std::endl; }

    virtual std::shared_ptr<Room> Clone() { return std::make_shared<BombedRoom>(*this); }

private:
    bool bombed_;
};

class Maze {
public:
    Maze() { std::cout << "Maze()" << std::endl; }
    Maze(const Maze& other) {
        std::cout << "Copy Maze()" << std::endl;
        rooms_.clear();
        for (auto it = other.rooms_.begin(); it != other.rooms_.end(); it++) {
            std::shared_ptr<Room> room = (*it)->Clone();
            room->Init((*it)->GetId());
            rooms_.emplace_back(room);
        }
    }
    ~Maze() { std::cout << "~Maze()" << std::endl; }

    virtual std::shared_ptr<Maze> Clone() { return std::make_shared<Maze>(*this); }
    void AddRoom(const std::shared_ptr<Room>& room) {
        std::cout << "AddRoom() Room id: " << room->GetId() << std::endl;
        rooms_.emplace_back(room);
    }

private:
    std::vector<std::shared_ptr<Room>> rooms_;
};

class MazeFactory {
public:
    ~MazeFactory() {}
    virtual std::shared_ptr<Maze> MakeMaze() {}

    virtual std::shared_ptr<Room> MakeRoom(int id) {}
};

class MazePrototypeFactory : public MazeFactory {
public:
    MazePrototypeFactory(const std::shared_ptr<Maze>& maze, const std::shared_ptr<Room>& room)
        : prototype_maze_(maze), prototype_room_(room) {}

    virtual std::shared_ptr<Maze> MakeMaze() { return prototype_maze_->Clone(); }

    virtual std::shared_ptr<Room> MakeRoom(int id) {
        std::shared_ptr<Room> room = prototype_room_->Clone();
        room->Init(id);
        return room;
    }

private:
    std::shared_ptr<Maze> prototype_maze_;
    std::shared_ptr<Room> prototype_room_;
};

class MazeGame {
public:
    std::shared_ptr<Maze> CreateMaze(MazeFactory& factory) {
        std::shared_ptr<Maze> maze = factory.MakeMaze();
        std::shared_ptr<Room> room1 = factory.MakeRoom(1);
        std::shared_ptr<Room> room2 = factory.MakeRoom(2);
        maze->AddRoom(room1);
        maze->AddRoom(room2);
        return maze;
    }
};

int main() {
    std::shared_ptr<Maze> maze = std::make_shared<Maze>();
    maze->AddRoom(std::make_shared<Room>(3));

    MazePrototypeFactory factory(maze, std::make_shared<BombedRoom>());
    MazeGame game;
    game.CreateMaze(factory);
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// Maze()
// Room()
// Room id: 3
// AddRoom() Room id: 3
// Prototype Room()
// Room id: 0
// Prototype BombedRoom()
// Copy Maze()
// Copy Room()
// Copy Room()
// Copy BombedRoom()
// Copy Room()
// Copy BombedRoom()
// AddRoom() Room id: 1
// AddRoom() Room id: 2
// ~Maze()
// ~Room()
// ~BombedRoom()
// ~Room()
// ~BombedRoom()
// ~Room()
// ~BombedRoom()
// ~Room()
// ~Maze()
// ~Room()
```

　　与浅拷贝的实现不同，在Maze类中拷贝构造函数中，other.rooms_通过每一个元素复制自身创建新的对象，并将指针放入rooms_中。  