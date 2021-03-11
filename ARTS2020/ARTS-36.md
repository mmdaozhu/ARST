---
layout: post
title: ARST(三十六)
date: 2020-12-21 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=orange>Algorithm</font>

#### **【LeetCode:338. Counting Bits】**

##### 　　Description:
　　Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

##### 　　Example 1:
```sh
Input: 2
Output: [0,1,1]
```

##### 　　Example 2:
```sh
Input: 5
Output: [0,1,1,2,1,2]
```

##### 　　C++ Solution 1:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/338.CountingBits/CountingBits1.cpp  

##### 　　C++ Solution 2:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/338.CountingBits/CountingBits2.cpp  

###  <font color=orange>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/c-20-the-library  
　　C++20: The Library  

#### **【Words】**
English | Chinese
-|-
chrono  | abbr. 按时间顺序的
decay | v. 衰退，衰减；衰败；腐烂，腐朽

#### **【Comments and Conclusions】**

##### Overview of C++20 library.
![hello](/ARTS2020/ARTS-36/1.png)

##### Calendar and Time-Zone
　　1. Calendar  

　　Calendar: consists of types, which represent a year, a month, a day of a weekday, and an n-th weekday of a month. This elementary types can be combined to complex types such for example year_month, year_month_day, year_month_day_last, years_month_weekday, and year_month_weekday_last. The operator "/" is overloaded for the convenient specification of time points. Additionally, we will get with C++20 new literals: d for a day and y for a year.  

　　2. Time-Zone  

　　Time-points can be displayed in various specific time-zones.  

##### std::span
　　A std::span stands for an object than can refer to a contiguous sequence of objects. A std::span, sometimes also called a view is never an owner. This contiguous memory can be an array, a pointer with a size, or a std::vector.The main reason for having a std::span<T> is that a plain array will be decay to a pointer if passed to a function; therefore, the size is lost.  
```c++
#include <iostream>
#include <span>

template <typename T>
void copy_n(const T* p, T* q, int n) {}

template <typename T>
void copy(std::span<const T> src, std::span<T> des) {}

int main() {
    int arr1[] = {1, 2, 3};
    int arr2[] = {3, 4, 5};

    copy_n(arr1, arr2, 3);
    copy<int>(arr1, arr2);
    return 0;
}
```

##### constexpr Containers
　　Before C++20, both can not be used in a constexpr evaluation, because there were three limiting aspects.  

　　- Destructors couldn't be constexpr.  
　　- Dynamic memory allocation/deallocation wasn't available.  
　　- In-place construction using placement-new wasn't available.  

　　These limiting aspects are now solved.  

　　Point 3 talks about placement-new, which is quite unknown. Placement-new is often used to instantiate an object in a pre-reserved memory area. Besides, you can overload placement-new globally or for your data types.  
```c++
char* memory = new char[sizeof(Account)];        // allocate memory
Account* account = new(memory) Account;          // construct in-place
account->~Account();                             // destruct
delete [] memory;                                // free memory
```
　　Here are the steps to use placement-new. The first line allocates memory for an Account, which is used in the second line to construct an account in-place. Admittedly, the expression account->~Account() looks strange. This expression is one of these rare cases, in which you have to call the destructor explicitly. Finally, the last line frees the memory.  

##### std::format
　　The text formatting library offers a safe and extensible alternative to the printf family of functions. It is intended to complement the existing C++ I/O streams library and reuse some of its infrastructure such as overloaded insertion operators for user-defined types.  

　　This concise description includes a straightforward example:  
```c++
std::string message = std::format("The answer is {}.", 42);
```

###  <font color=orange>tips: two-phase locking(2pl)</font>
　　two-phase locking: a pessimistic concurrency control protocol that uses locks to determine whether a transaction is allowed to access an object in the database.  

#### **【Phase 1– Growing】**
　　In the growing phase, each transaction requests the locks that it need from the DBMS's lock manager. The lock manager grants/denies these lock requests.  

#### **【Phase 2– Shrinking】**
　　Transactions enter the shrinking phase immediately after it releases its first lock. In the shrinking phase, transactions are only allowed to release locks. They are not allowed to acquire new ones.  

###  <font color=orange>Share: 浅谈装饰器模式</font>

#### **【什么是装饰器模式？】**
　　《设计模式》一书中是这样定义的：动态地给一个对象增加一些额外地职责。就增加功能来说，Decorator模式相比生成子类更为灵活。  

　　我们来看下装饰器模式的类图：  
![hello](/ARTS2020/ARTS-36/2.jpg)

#### **【装饰器模式的优点】**
　　1. 比静态继承更灵活：采用组合的方式灵活地向对象增加职责。可以用增加和分离的方法，在运行时增加和删除职责。  

　　2. 避免在层次结构高层的类有太多的特征。  

#### **【装饰器模式的应用场景】**
　　1. 在不影响其他对象的情况下，以动态、透明的方式给单个对象增加职责。  

　　2. 处理那些可以撤销的职责。  

#### **【装饰器模式的实现方法】**
　　我们直接看代码实现：  
```c++
#include <iostream>
#include <memory>

class VisualComponent {
public:
    VisualComponent() = default;
    virtual ~VisualComponent() {}

    virtual void Draw() = 0;
};

class TextView : public VisualComponent {
public:
    TextView() = default;
    ~TextView() = default;

    virtual void Draw() { std::cout << "TextView::Draw()" << std::endl; }
};

class Decorator : public VisualComponent {
public:
    Decorator(const std::shared_ptr<VisualComponent>& component_ptr) : component_ptr_(component_ptr) {}
    virtual ~Decorator() {}

    virtual void Draw() { component_ptr_->Draw(); }

private:
    std::shared_ptr<VisualComponent> component_ptr_;
};

class BorderDecorator : public Decorator {
public:
    BorderDecorator(const std::shared_ptr<VisualComponent>& component_ptr, int border_width)
        : Decorator(component_ptr), border_width_(border_width) {}
    ~BorderDecorator() = default;

    virtual void Draw() {
        Decorator::Draw();
        DrawBorder();
    }

private:
    void DrawBorder() { std::cout << "BorderDecorator::DrawBorder() border_width_: " << border_width_ << std::endl; }

private:
    int border_width_;
};

int main() {
    std::shared_ptr<VisualComponent> componet = std::make_shared<BorderDecorator>(std::make_shared<TextView>(), 1);
    componet->Draw();
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// TextView::Draw()
// BorderDecorator::DrawBorder() border_width_: 1
```

　　VisualComponent是一个对象的抽象接口，可以给这些对象动态的增加职责。  

　　TextView和Decorator类实现VisualComponent接口，Decorator类并且持有一个实现VisualComponent的对象，这里是TextView类的对象。  

　　BorderDecorator类中的Draw()成员函数中，先执行TextView的Draw()函数，然后附加自己的DrawBorder()职责。  

