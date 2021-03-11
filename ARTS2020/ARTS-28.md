---
layout: post
title: ARST(二十八)
date: 2020-10-26 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=purple>Algorithm</font>

#### **【LeetCode:235. Lowest Common Ancestor of a Binary Search Tree】**

##### 　　Description:
　　Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.  

　　According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”  

##### 　　Example 1:
![hello](/ARTS2020/ARTS-28/1.png)
```sh
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

##### 　　Example 2:
![hello](/ARTS2020/ARTS-28/2.png)
```sh
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

##### 　　Example 3:
```sh
Input: root = [1,2], p = 1, q = 2
Output: 1
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/235.LowestCommonAncestorOfABinarySearchTree/LowestCommonAncestorOfABinarySearchTree.cpp  

###  <font color=purple>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
conjunction | n. 结合

#### **【Comments and Conclusions】**

##### Caching at the database query level
　　Whenever you query the database, hash the query as a key and store the result to the cache. This approach suffers from expiration issues:  

　　- Hard to delete a cached result with complex queries  
　　- If one piece of data changes such as a table cell, you need to delete all cached queries that might include the changed cell  
　
##### Caching at the object level
　　See your data as an object, similar to what you do with your application code. Have your application assemble the dataset from the database into a class instance or a data structure(s)  

　　Suggestions of what to cache:  

　　- User sessions  
　　- Fully rendered web pages  
　　- Activity streams  
　　- User graph data  

##### When to update the cache(Cache-aside)
　　The application is responsible for reading and writing from storage. The cache does not interact with storage directly. The application does the following:  

　　- Look for entry in cache, resulting in a cache miss  
　　- Load entry from the database  
　　- Add entry to cache  
　　- Return entry  

　　Disadvantage(s): cache-aside  

　　- Each cache miss results in three trips, which can cause a noticeable delay.  
　　- Data can become stale if it is updated in the database. This issue is mitigated by setting a time-to-live (TTL) which forces an update of the cache entry, or by using write-through.  
　　- When a node fails, it is replaced by a new, empty node, increasing latency.  

##### When to update the cache(Write-through)
　　The application uses the cache as the main data store, reading and writing data to it, while the cache is responsible for reading and writing to the database:  

　　- Application adds/updates entry in cache  
　　- Cache synchronously writes entry to data store  
　　- Return  

　　Disadvantage(s): write through  

　　- When a new node is created due to failure or scaling, the new node will not cache entries until the entry is updated in the database. Cache-aside in conjunction with write through can mitigate this issue.  
　　- Most data written might never be read, which can be minimized with a TTL.  

##### When to update the cache(write-behind)
　　In write-behind, the application does the following:  

　　- Add/update entry in cache  
　　- Asynchronously write entry to the data store, improving write performance  

　　Disadvantage(s): write-behind  

　　- There could be data loss if the cache goes down prior to its contents hitting the data store.  
　　- It is more complex to implement write-behind than it is to implement cache-aside or write-through.  

##### When to update the cache(Refresh-ahead)
　　You can configure the cache to automatically refresh any recently accessed cache entry prior to its expiration.  

　　Refresh-ahead can result in reduced latency vs read-through if the cache can accurately predict which items are likely to be needed in the future.  

　　Disadvantage(s): refresh-ahead  

　　- Not accurately predicting which items are likely to be needed in the future can result in reduced performance than without refresh-ahead.  


###  <font color=purple>tips: 程序员如何避免面试被坑，斩获满意Offer</font>
https://www.bilibili.com/video/BV1L7411j7CN  

##### 有一份出色的简历
　　1. 自我介绍：主业、领导力、专业与专攻、行业背景、职业、业务场景  

　　2. 个人信息：github、博客展示、个人作品、奖项  

　　3. 个人技能：技术技能栈、所在的技术领域、曾经做过的业务领域、经验和软技能  

　　4. 工作经历与教育经历  

##### 准备算法

##### 如何准备工作项目
　　1. 分享你做过的最自豪的项目  

　　2. 解决过的最难的技术问题？  

　　3. 做过最痛苦或者艰难的项目？  

　　4. 分享犯过的最大的错误和技术故障  

　　讲故事：STAR，Situation，Task，Action，Result  

###  <font color=purple>Share: 浅谈工厂方法模式</font>

#### **【什么是工厂方法？】**
　　《设计模式》一书中是这样定义的：定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使一个类的实例化延迟到其子类。  

　　工厂方法封装了对象的创建，客户无需关心对象的创建过程就能够获取相应的对象。  

　　我们来看下工厂方法的类图：  
![hello](/ARTS2020/ARTS-28/3.jpg)

　　客户通过Creator的子类ConcreteCreator提供的AnOperation()接口，委派FactoryMethod()方法实例化特定的ConcreteProduct子类。如果客户需要ConcreteProductB子类，我们就不得不创建Creator的子类ConcreteCreatorB。不过在c++中可以使用模板避免。  

#### **【工厂方法的应用场景】**
　　1. 当代码中存在if-else分支判断，动态地根据不同的类型创建不同的对象。  

　　2. 当单个对象本身的创建逻辑比较复杂的时候。  

#### **【工厂方法的实现方法】**

##### 简单工厂：
　　将多个对象的创建逻辑放在一个工厂类中，这就是所谓的简单工厂。  

　　具体看如下代码：  
```c++
#include <iostream>
#include <memory>

class Product {
public:
    virtual ~Product(){}
    virtual void print() { std::cout << "print Product" << std::endl; }
};

class MyProduct : public Product {
public:
    virtual void print() { std::cout << "print MyProduct" << std::endl; }
};

class YourProduct : public Product {
public:
    virtual void print() { std::cout << "print YourProduct" << std::endl; }
};

class Creator {
public:
    std::shared_ptr<Product> Create(int productid) {
        if (productid == 0) {
            return std::make_shared<MyProduct>();
        } else if (productid == 1) {
            return std::make_shared<YourProduct>();
        } else {
            return std::make_shared<Product>();
        }
    }
};

int main() {
    Creator creator;
    std::shared_ptr<Product> my_product = creator.Create(0);
    my_product->print();
    std::shared_ptr<Product> your_product = creator.Create(1);
    your_product->print();
    std::shared_ptr<Product> default_product = creator.Create(2);
    default_product->print();
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// print MyProduct
// print YourProduct
// print Product
```

　　在Creator类的成员方法Create()中，根据标识参数来创建不同的对象。  

　　简单工厂一般用于每个对象的创建逻辑都比较简单的时候。  

##### 工厂方法：
　　当单个对象本身的创建逻辑比较复杂的时候，我们就会考虑使用工厂模式。将单个对象的创建逻辑封装在一个工厂类中。  

　　具体看如下代码：  
```c++
#include <iostream>
#include <memory>

class Maze {
public:
    virtual ~Maze(){}
    Maze() { std::cout << "Maze()" << std::endl; }
};

class BombedMaze : public Maze {
public:
    BombedMaze() { std::cout << "BombedMaze()" << std::endl; }
};

class EnchantedMaze : public Maze {
public:
    EnchantedMaze() { std::cout << "EnchantedMaze()" << std::endl; }
};

class MazeGame {
public:
    std::shared_ptr<Maze> CreateMaze() {
        std::cout << "start creating" << std::endl;
        std::cout << "finish creating" << std::endl;
        return MakeMaze();
    }

protected:
    virtual std::shared_ptr<Maze> MakeMaze() { return std::make_shared<Maze>(); }
};

class BombedMazeGame : public MazeGame {
protected:
    virtual std::shared_ptr<Maze> MakeMaze() { return std::make_shared<BombedMaze>(); }
};

class EnchantedMazeGame : public MazeGame {
protected:
    virtual std::shared_ptr<Maze> MakeMaze() { return std::make_shared<EnchantedMaze>(); }
};

int main() {
    MazeGame maze_game;
    std::shared_ptr<Maze> maze = maze_game.CreateMaze();

    BombedMazeGame bombed_maze_game;
    std::shared_ptr<Maze> bombed_maze = bombed_maze_game.CreateMaze();

    EnchantedMazeGame enchanted_maze_game;
    std::shared_ptr<Maze> enchanted_maze = enchanted_maze_game.CreateMaze();
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// start creating
// finish creating
// Maze()
// start creating
// finish creating
// Maze()
// BombedMaze()
// start creating
// finish creating
// Maze()
// EnchantedMaze()
```

　　在这个例子中，我们定义一个父类Maze类和一个父类MazeGame类，MazeGame类用于创建Maze类。  

　　如果需要新增一个Maze子类，比如BombedMaze类，我们就需要相应的定义子类BombedMazeGame类。  

　　由此可见，工厂模式的类层次是连接平行的。  

##### 模板实现的工厂方法：
　　工厂方法有个缺点，如果想新增一个Maze的子类，就迫使需要创建一个MazeGame的子类。  

　　这时候我们提供一个MazeGame的模板子类StandardMazeGame，它使用Maze的子类作为模板参数。  

　　具体看如下代码：  
```c++
#include <iostream>
#include <memory>

class Maze {
public:

    Maze() { std::cout << "Maze()" << std::endl; }
};

class BombedMaze : public Maze {
public:
    BombedMaze() { std::cout << "BombedMaze()" << std::endl; }
};

class EnchantedMaze : public Maze {
public:
    EnchantedMaze() { std::cout << "EnchantedMaze()" << std::endl; }
};

class MazeGame {
public:
    std::shared_ptr<Maze> CreateMaze() {
        std::cout << "start creating" << std::endl;
        std::cout << "finish creating" << std::endl;
        return MakeMaze();
    }

protected:
    virtual std::shared_ptr<Maze> MakeMaze() { return std::make_shared<Maze>(); }
};

template <typename T>
class StandardMazeGame : public MazeGame {
protected:
    virtual std::shared_ptr<Maze> MakeMaze() { return std::make_shared<T>(); }
};

int main() {
    MazeGame maze_game;
    std::shared_ptr<Maze> maze = maze_game.CreateMaze();

    StandardMazeGame<BombedMaze> bombed_maze_game;
    std::shared_ptr<Maze> bombed_maze = bombed_maze_game.CreateMaze();

    StandardMazeGame<EnchantedMaze> enchanted_maze_game;
    std::shared_ptr<Maze> enchanted_maze = enchanted_maze_game.CreateMaze();
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// start creating
// finish creating
// Maze()
// start creating
// finish creating
// Maze()
// BombedMaze()
// start creating
// finish creating
// Maze()
// EnchantedMaze()
```

