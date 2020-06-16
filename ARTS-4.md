---
layout: post
title: ARST(四)
date: 2020-05-11 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=red>Algorithm</font>

#### **【LeetCode:141. Linked List Cycle】**

##### 　　Description:
　　Given a linked list, determine if it has a cycle in it.  
　　To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.  

##### 　　Example 1:
```sh
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```
![hello](/ARTS-4/1.png)

##### 　　Example 2:
```sh
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```
![hello](/ARTS-4/2.png)

##### 　　Example 3:
```sh
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```
![hello](/ARTS-4/3.png)

##### 　　C++ Solution 1:
```cpp
/*
解体思路：
   将链表每个结点的指针依次在set里面找下，如果存在返回true，不存在就把放到set里面。

时间复杂度分析：O(logn)
*/

#include <assert.h>

#include <iostream>
#include <set>

/**
 * Definition for singly-linked list.
 *
 */
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    bool hasCycle(ListNode* head) {
        std::set<ListNode*> s;
        ListNode* p = head;
        while (p != nullptr) {
            if (s.find(p) != s.end()) {
                return true;
            } else {
                s.insert(p);
            }
            p = p->next;
        }
        return false;
    }
};

```

##### 　　C++ Solution 2:
```cpp
/*
解体思路：
    使用快慢指针

时间复杂度分析：O(logn)
*/

#include <assert.h>

#include <iostream>

/**
 * Definition for singly-linked list.
 *
 */

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class Solution {
public:
    bool hasCycle(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                return true;
            }
        }
        return false;
    }
};

```

###  <font color=red>Review</font>

#### **【Article】**
http://www.aosabook.org/en/distsys.html (1.3. The Building Blocks of Fast and Scalable Data Access[Caches, Global Cache])

#### **【Words】**

English | Chinese
-|-
translate | vt. 翻译；转化；解释；转变为；调动
moreover | adv. 而且；此外
intermediate | adj. 中间的，过渡的；中级的，中等的
overwhelm | vt. 淹没；压倒；受打击；覆盖；压垮
eviction | n. 逐出；赶出；收回


#### **【Comments and Conclusions】**
　　When talking about scaling access to the data, there are many options that you can employ to make this easier: four of the more important ones are caches, proxies, indexex and load balancers.  
　　Caches chart:  
![hello](/ARTS-4/4.png)
　　1.Inserting a cache on your request layer node  

![hello](/ARTS-4/5.png)
　　2.Multiple caches  

![hello](/ARTS-4/6.png)
　　3.Global Cache where cache is responsible for retrieval  

![hello](/ARTS-4/7.png)	
　　4.Global Cache where request nodes are responsible for retrieval  

###  <font color=red>Tip: VS Code中C++ formatting</font>
　　add this setting to User Settings settings.json  
```sh
{
    "C_Cpp.clang_format_style": "{BasedOnStyle: Google, IndentWidth: 4, AccessModifierOffset: -4, AlignTrailingComments: true, ColumnLimit: 120, ConstructorInitializerAllOnOneLineOrOnePerLine: true}"
}
```
　　Use shortcut key(shift + alt + F) to format codes.  
　　I prefer this formatting recently.  

###  <font color=red>Share: C++实现策略模式</font>

#### **【古典的策略模式】**
　　策略类比较简单，包括策略的接口和实现策略的类。这样可以使算法的<font color=red>变化</font>独立于客户端，客户端代码基于接口而非实现编程。  
　　示例代码如下：  
```c++
// strategy1.h

class GameCharacter;

class HealthCalcFunc {
public:
    virtual int calc(const GameCharacter& gc) const = 0;
};

class SlowHealthLoser : public HealthCalcFunc {
public:
    virtual int calc(const GameCharacter& gc) const;
};

class FastHealthLoser : public HealthCalcFunc {
public:
    virtual int calc(const GameCharacter& gc) const;
};

class GameCharacter {
public:
    explicit GameCharacter(HealthCalcFunc* hcf);
    int healthValue() const { return healthFunc->calc(*this); }

private:
    HealthCalcFunc* healthFunc;
};
```

```c++
// strategy1.cpp

#include "strategy1.h"

#include <iostream>

int SlowHealthLoser::calc(const GameCharacter& gc) const {
    std::cout << "SlowHealthLoser::calc" << std::endl;
    return 0;
}

int FastHealthLoser::calc(const GameCharacter& gc) const {
    std::cout << "FastHealthLoser::calc" << std::endl;
    return 0;
}

GameCharacter::GameCharacter(HealthCalcFunc* hcf) : healthFunc{hcf} {
    std::cout << "Constructor of GameCharacter" << std::endl;
}

int main() {
    GameCharacter guy1{new SlowHealthLoser{}};
    guy1.healthValue();
    GameCharacter guy2{new FastHealthLoser{}};
    guy2.healthValue();
    return 0;
}

// g++ --std=c++11 -Wall strategy1.cpp -o strategy1
// result:
// Constructor of GameCharacter
// SlowHealthLoser::calc
// Constructor of GameCharacter
// FastHealthLoser::calc
```
　　这种实现策略模式的方式很容易辨认。如果我们想新增一种计算健康状态的算法，只需要新增一个HealthCalcFunc类的派生类即可。  

#### **【使用函数指针实现策略模式】**
　　和古典的策略模式类似，以策略函数的声明为接口，各种策略函数实现为实现，将接口传递给客户端使用，客户端可以使用符合接口定义的各种策略函数来满足自己的需求。  
　　示例代码如下：  
```c++
// strategy2.h

class GameCharacter;

int defaultHealthCalc(const GameCharacter& gc);

class GameCharacter {
public:
    typedef int (*HealthCalcFunc)(const GameCharacter&);
    explicit GameCharacter(HealthCalcFunc hcf = defaultHealthCalc);
    int healthValue() const { return healthFunc(*this); }

private:
    HealthCalcFunc healthFunc;
};

class EvilBadGuy : public GameCharacter {
public:
    explicit EvilBadGuy(HealthCalcFunc hcf = defaultHealthCalc);
};
```

```c++
// strategy2.cpp

#include "strategy2.h"

#include <iostream>

int defaultHealthCalc(const GameCharacter& gc) {
    std::cout << "defaultHealthCalc" << std::endl;
    return 0;
}

int loseHealthQuickly(const GameCharacter& gc) {
    std::cout << "loseHealthQuickly" << std::endl;
    return 0;
}

int loseHealthSlowly(const GameCharacter& gc) {
    std::cout << "loseHealthSlowly" << std::endl;
    return 0;
}

GameCharacter::GameCharacter(HealthCalcFunc hcf) : healthFunc{hcf} {
    std::cout << "Constructor of GameCharacter" << std::endl;
}

EvilBadGuy::EvilBadGuy(HealthCalcFunc hcf) : GameCharacter{hcf} {
    std::cout << "Constructor of EvilBayGuy" << std::endl;
}

int main() {
    EvilBadGuy guy1{};
    guy1.healthValue();
    EvilBadGuy guy2{loseHealthQuickly};
    guy2.healthValue();
    EvilBadGuy guy3{loseHealthSlowly};
    guy3.healthValue();
    return 0;
}

// g++ --std=c++11 -Wall strategy2.cpp -o strategy2
// result:
// Constructor of GameCharacter
// Constructor of EvilBayGuy
// defaultHealthCalc
// Constructor of GameCharacter
// Constructor of EvilBayGuy
// loseHealthQuickly
// Constructor of GameCharacter
// Constructor of EvilBayGuy
// loseHealthSlowly
```
　　只要满足typedef int (\*HealthCalcFunc)(const GameCharacter&);函数声明的都可以供客户端使用。  

#### **【使用tr1::function实现策略模式】**
　　使用函数指针实现策略模式过于死板，为啥不能用函数对象，成员函数来作为策略函数呢？  
　　如果我们使用tr1::function，这些约束就不见了，只要策略函数的签名式兼容于需求端即可。这样代码就非常的灵活。  
　　示例代码如下： 
```c++
// strategy3.h

#include <iostream>
#include <tr1/functional>

class GameCharacter;

int defaultHealthCalc(const GameCharacter& gc);

struct HealthCalculator {
    int operator()(const GameCharacter& gc) const {
        std::cout << "HealthCalculator function object" << std::endl;
        return 0;
    }
};

class GameLevel {
public:
    float health(const GameCharacter& gc) const;
};

class GameCharacter {
public:
    typedef std::tr1::function<int(const GameCharacter&)> HealthCalcFunc;
    explicit GameCharacter(HealthCalcFunc hcf = defaultHealthCalc);
    int healthValue() const { return healthFunc(*this); }

private:
    HealthCalcFunc healthFunc;
};

class EvilBadGuy : public GameCharacter {
public:
    explicit EvilBadGuy(HealthCalcFunc hcf = defaultHealthCalc);
};

class EvilCandyCharacter : public GameCharacter {
public:
    explicit EvilCandyCharacter(HealthCalcFunc hcf = defaultHealthCalc);
};
```

```c++
// strategy3.cpp

#include "strategy3.h"

#include <iostream>

int defaultHealthCalc(const GameCharacter& gc) {
    std::cout << "defaultHealthCalc" << std::endl;
    return 0;
}

short calcHealth(const GameCharacter& gc) {
    std::cout << "calcHealth" << std::endl;
    return 0;
}

float GameLevel::health(const GameCharacter& gc) const {
    std::cout << "GameLevel::health" << std::endl;
    return 0;
}

GameCharacter::GameCharacter(HealthCalcFunc hcf) : healthFunc{hcf} {
    std::cout << "Constructor of GameCharacter" << std::endl;
}

EvilBadGuy::EvilBadGuy(HealthCalcFunc hcf) : GameCharacter{hcf} {
    std::cout << "Constructor of EvilBayGuy" << std::endl;
}

EvilCandyCharacter::EvilCandyCharacter(HealthCalcFunc hcf) : GameCharacter{hcf} {
    std::cout << "Constructor of EvilCandyCharacter" << std::endl;
}

int main() {
    EvilBadGuy guy{};
    guy.healthValue();
    EvilBadGuy guy1{calcHealth};
    guy1.healthValue();
    EvilCandyCharacter ecc{HealthCalculator{}};
    ecc.healthValue();

    GameLevel currentLevel{};
    EvilCandyCharacter ecc1{std::tr1::bind(&GameLevel::health, currentLevel, std::tr1::placeholders::_1)};
    ecc1.healthValue();
    return 0;
}

//  g++ --std=c++11 -Wall strategy3.cpp -o strategy3
// result:
// Constructor of GameCharacter
// Constructor of EvilBayGuy
// defaultHealthCalc
// Constructor of GameCharacter
// Constructor of EvilBayGuy
// calcHealth
// Constructor of GameCharacter
// Constructor of EvilCandyCharacter
// HealthCalculator function object
// Constructor of GameCharacter
// Constructor of EvilCandyCharacter
// GameLevel::health
```
　　由tr1::function类型产生的对象可以持有任何与此签名是兼容的可调物。兼容就是这个可调物的参数可被隐式转换为const GameCharacter&，而其返回类型可被隐式转换为int。  
　　接下来看下tr1::bind做的事情，GameLevel::health宣称自己接受一个参数，其实它接受两个参数，被忽略的是第一个参数，隐式的this指针。然而GameCharacter的健康计算函数只接受一个参数，如果我们要使用GameLevel::health作为健康计算函数，必须以某种方式来转换它，使他不再接受两个参数。于是我们将currentLevel绑定为GameLevel的对象，使得它在”每次GameLevel::health被调用时“使用。这正是tr1::bind的功劳。  