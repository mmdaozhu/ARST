---
layout: post
title: ARST(三十五)
date: 2020-12-14 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=orange>Algorithm</font>

#### **【LeetCode:212. Word Search II】**

##### 　　Description:
　　Given an m x n board of characters and a list of strings words, return all words on the board.  

　　Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.  

##### 　　Example 1:
![hello](/ARTS2020/ARTS-35/1.jpg)
```sh
Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]
```

##### 　　Example 2:
![hello](/ARTS2020/ARTS-35/2.jpg)
```sh
Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/212.WordSearchII/WordSearchII.cpp  


###  <font color=orange>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/c-20-the-core-language  
　　C++ 20: The Core Language  

#### **【Words】**
English | Chinese
-|-
sophisticated | adj. 复杂的；精致的

#### **【Comments and Conclusions】**

##### Overview of the core language
![hello](/ARTS2020/ARTS-35/4.png)

##### The three-way Comparison operator <=>
　　The three-way comparison operator <=> is often just called spaceship operator. The spaceship operator determines for two values A  and B whether A < B, A = B, or A > B.  

　　The compiler can auto-generate the three-way comparison operator. You have only to ask for it politely with default. In this case you get all six comparison operators such as ==, !=, <, <=, >, and >=.  

　　Here is the code-snippet.  
```c++
#include <iostream>

struct Basics {
    int i;
    char c;
    float f;
    double d;
    auto operator<=>(const Basics&) const = default;
};

struct Arrays {
    int ai;
    char ac;
    float af;
    double ad;
    auto operator<=>(const Arrays&) const = default;
};

struct Bases : Basics, Arrays {
    auto operator<=>(const Bases&) const = default;
};

int main() {
    constexpr Bases a = {{0, 'c', 1.f, 1.}, {1, 'a', 1.f, 3.}};
    constexpr Bases b = {{0, 'c', 1.f, 1.}, {1, 'a', 1.f, 3.}};

    static_assert(a == b);
    static_assert(!(a != b));
    static_assert(!(a < b));
    static_assert(a <= b);
    static_assert(!(a > b));
    static_assert(a >= b);
}
```

##### String Literals as Template Parameters
　　Before C++20, you can not use a string as a non-type template parameter. With C++20, you can use it. The idea is to use the in the standard defined basic_fixed_string, which has a constexpr constructor. The constexpr constructor enables it to instantiate the fixed string at compile-time.  
```c++
#include <algorithm>
#include <iostream>

template <int N>
struct basic_fixed_string {
    constexpr basic_fixed_string(const char (&name)[N]) { std::copy_n(name, N, data); }
    char data[N]{};
};

template <basic_fixed_string T>
class Foo {
    static constexpr char const* Name = T.data;

public:
    void hello() const { std::cout << Name << std::endl; }
};

int main() {
    Foo<"Hello!"> foo;
    foo.hello();
}
```

##### constexpr Virtual Functions
　　Virtual functions can not be invoked in constant expressions because the dynamic type is not known. This restriction will fall with C++20.  

##### Designated initializers
　　Here designated initializers from C99 kick in.  
```c++
#include <iostream>

struct Point2D {
    int x;
    int y;
};

class Point3D {
public:
    int x;
    int y;
    int z;
};

int main() {
    Point2D point2D{.x = 1, .y = 2};
    // Point2D point2d {.y = 2, .x = 1};         // (1) error
    Point3D point3D{.x = 1, .y = 2, .z = 2};
    // Point3D point3D {.x = 1, .z = 2}          // (2)  {1, 0, 2}

    std::cout << "point2D: " << point2D.x << " " << point2D.y << std::endl;
    std::cout << "point3D: " << point3D.x << " " << point3D.y << " " << point3D.z << std::endl;
    std::cout << std::endl;
}
```

##### Various Lambda Improvements
　　1. Allow [=, this] as lambda capture and deprecate implicit capture of this via [=]  
```c++
#include <iostream>

struct Lambda {
    auto foo() {
        return [=] { std::cout << s << std::endl; };
    }
    std::string s;
};

struct LambdaCpp20 {
    auto foo() {
        return [=, this] { std::cout << s << std::endl; };
    }
    std::string s;
};

int main() {
    return 0;
}
```
　　The implicit [=] capture by copy inside the struct Lambda causes in C++20 a deprecation warning.   


　　2. Template lambdas  

　　Sometimes, you want to define a lambda that works only for a specific type such as a std::vector. Now, template lambdas come to our rescue. Instead of a type parameter, you can also use a concept:  
```c++
#include <iostream>
#include <vector>

int main() {
    auto foo = []<typename T>(const std::vector<T>& vec) {
        for (auto const& v : vec) {
            std::cout << v << std::endl;
        }
    };

    std::vector<int> vec{0, 1, 2, 3, 4, 5};
    foo(vec);
    return 0;
}
```

##### New Attributes: [[likely]] and [[unlikely]]
　　With C++20, we get new attributes [[likely]] and [[unlikely]]. Both attributes allow it to give the optimiser a hint, whether path of execution is more or less likely.  
```c++
#include <iostream>
#include <vector>
#include <cmath>

int main() {
    std::vector<int> v{0, 1, 2, 3, 4, 5};
    double sum = 0.0;
    for (size_t i = 0; i < v.size(); ++i) {
        if (v[i] < 0) {
            [[likely]] sum -= sqrt(-v[i]);
        } else {
            sum += sqrt(v[i]);
        }
    }
    std::cout << sum << std::endl;
    return 0;
}
```

##### consteval and constinit Specifier
　　The new specifier consteval creates an immediate function. For an immediate function, every call to the function must produce a compile-time constant expression. An immediate function is implicit a constexpr function.  
```c++
#include <iostream>

consteval int sqr(int n) { return n * n; }

int main() {
    constexpr int r = sqr(100);  // OK

    int x = 100;
    int r2 = sqr(x);  // Error
    return 0;
}
```

　　constinit ensures that the variable with static storage duration is initialized at compile-time.    

##### std::experimental::source_location
　　C++11 has two macros for \_\_LINE\_\_ and \_\_FILE\_\_ to get the information when the macros are used. With C++20, the class source_location gives you the file name, the line number, column number, and function name about the source code.   
```c++
#include <experimental/source_location>
#include <iostream>
#include <string_view>

void log(std::string_view message,
         const std::experimental::source_location& location = std::experimental::source_location::current()) {
    std::cout << "info:" << location.file_name() << ":" << location.line() << " " << message << '\n';
}

int main() {
    log("Hello world!");  // info:main.cpp:12 Hello world!
    return 0;
}
```


###  <font color=orange>tips: Unix哲学</font>
　　1. 模块原则：使用简洁的接口拼合简单的部件。  

　　2. 清晰原则：清晰胜于机巧。  

　　3. 组合原则：设计时考虑拼接组合。  

　　4. 分离原则：策略同机制分离，接口同引擎分离。  

　　5. 简洁原则：设计要简洁，复杂度能低则低。  

　　6. 吝啬原则：除非确无他法，不要编写庞大的程序。  

　　7. 透明性原则：设计要可见，以便审查和调试。  

　　8. 健壮原则：健壮源于透明与简洁。  

　　9. 表示原则：把知识叠入数据以求逻辑质朴而健壮。  

　　10. 通俗原则：接口设计避免标新立异。  

　　11. 缄默原则：如果一个程序没什么好说的，就沉默。  

　　12. 补救原则：出现异常时，马上退出并给出足够错误信息。  

　　13. 经济原则：宁可机器一分，不花程序员一秒。  

　　14. 生成原则：避免手工hack，尽量编写程序去生成程序。  

###  <font color=orange>Share: 浅谈桥接模式</font>

#### **【什么是桥接模式？】**
　　《设计模式》一书中是这样定义的：将抽象部分与它的实现部分分离，使它们可以独立地变化。  

　　我们来看下桥接模式的类图：  
![hello](/ARTS2020/ARTS-35/3.jpg)  

　　Abstraction是定义抽象类的接口，拥有一个执行Implementor类型对象的指针。  

　　Abstraction和Implementor通过组合方式分离，抽象部分的类层次结构和实现部分的类层次结构独立变化。  

#### **【桥接模式的优点】**
　　1. 分离接口及其实现部分：将Abstraction和Implementor分离有助于降低对实现部分编译时的依赖性。  

　　2. 提供可扩展性：独立地对Abstraction和Implementor部分进行扩展。  

　　3. 实现细节对客户透明。  

#### **【桥接模式的应用场景】**
　　1. 希望类的抽象和它的实现可以通过生成子类的方法进行扩充。  

　　2. 在多个对象间共享实现（使用引用计数），并对客户隐藏这一细节。  

#### **【桥接模式的实现方法】**
　　我们直接看代码实现：  
```c++
#include <iostream>
#include <memory>
#include <vector>

struct Point {
    int x;
    int y;
};

/**********************************WindowImp**********************************/
class WindowImp {
public:
    virtual ~WindowImp() {}
    virtual void DeviceRect(const Point& p1, const Point& p2) = 0;
};

class XWindowImp : public WindowImp {
public:
    virtual void DeviceRect(const Point& p1, const Point& p2) {
        std::cout << "XWindowImp::DeviceRect()" << std::endl;
        std::cout << "Draw p1 x: " << p1.x << " y: " << p1.y << std::endl;
        std::cout << "Draw p2 x: " << p2.x << " y: " << p2.y << std::endl;
    }
};

class PMWindowImp : public WindowImp {
public:
    virtual void DeviceRect(const Point& p1, const Point& p2) {
        std::cout << "PMWindowImp::DeviceRect()" << std::endl;
        std::cout << "Draw p2 x: " << p2.x << " y: " << p2.y << std::endl;
        std::cout << "Draw p1 x: " << p1.x << " y: " << p1.y << std::endl;
    }
};

/**********************************Window*************************************/
class Window {
public:
    Window(const std::string& name) : name(name) {}
    virtual ~Window() {}

    virtual void DrawRect(const Point& p1, const Point& p2) = 0;

protected:
    virtual std::shared_ptr<WindowImp> GetWindowImp() {
        if (!imp_) {
            if (name == "app") {
                return std::make_shared<XWindowImp>();
            } else if (name == "icon") {
                return std::make_shared<PMWindowImp>();
            }
        }
        return imp_;
    }

private:
    std::shared_ptr<WindowImp> imp_;
    std::string name;
};

class ApplicationWindow : public Window {
public:
    ApplicationWindow(const std::string& name) : Window(name) { std::cout << "ApplicationWindow()" << std::endl; }
    virtual void DrawRect(const Point& p1, const Point& p2) { GetWindowImp()->DeviceRect(p1, p2); }
};

class IconWindow : public Window {
public:
    IconWindow(const std::string& name) : Window(name) { std::cout << "IconWindow()" << std::endl; }
    virtual void DrawRect(const Point& p1, const Point& p2) { GetWindowImp()->DeviceRect(p1, p2); }
};

int main() {
    std::shared_ptr<Window> app_window = std::make_shared<ApplicationWindow>("app");
    std::shared_ptr<Window> icon_window = std::make_shared<IconWindow>("icon");
    Point p1(1, 2);
    Point p2(3, 4);
    app_window->DrawRect(p1, p2);
    icon_window->DrawRect(p1, p2);
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// ApplicationWindow()
// IconWindow()
// XWindowImp::DeviceRect()
// Draw p1 x: 1 y: 2
// Draw p2 x: 3 y: 4
// PMWindowImp::DeviceRect()
// Draw p2 x: 3 y: 4
// Draw p1 x: 1 y: 2
```

　　抽象部分是Window类层次关系，实现部分是WindowImp类层次关系，它们可以独立的扩展。  

　　GetWindowImp()成员函数中针对name来构造不同的WindowImp子类，当然这个地方也可以应用抽象工厂模式。  

　　当app_window和icon_window执行DrawRect()函数时，会分别调用XWindowImp::DeviceRect和PMWindowImp::DeviceRect方法，实现部分对客户透明。  
