---
layout: post
title: ARST(三十四)
date: 2020-12-07 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=orange>Algorithm</font>

#### **【LeetCode:37. Sudoku Solver】**

##### 　　Description:
　　Write a program to solve a Sudoku puzzle by filling the empty cells.  

　　A sudoku solution must satisfy all of the following rules:  

　　Each of the digits 1-9 must occur exactly once in each row.  
　　Each of the digits 1-9 must occur exactly once in each column.  
　　Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.  
　　The '.' character indicates empty cells.  

##### 　　Example:
```sh
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: 
[["5","3","4","6","7","8","9","1","2"]
,["6","7","2","1","9","5","3","4","8"]
,["1","9","8","3","4","2","5","6","7"]
,["8","5","9","7","6","1","4","2","3"]
,["4","2","6","8","5","3","7","9","1"]
,["7","1","3","9","2","4","8","5","6"]
,["9","6","1","5","3","7","2","8","4"]
,["2","8","7","4","1","9","6","3","5"]
,["3","4","5","2","8","6","1","7","9"]]
Explanation: The input board is shown above and the only valid solution is shown below:
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/037.SudokuSolver/SudokuSolver.cpp  

###  <font color=orange>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/thebigfour  
　　C++20: The Big Four  

#### **【Words】**
English | Chinese
-|-
tinkering | n. 铸补，熔补
cryptic | adj. 神秘的，含义模糊的
revolutionise | vt. 彻底改变（等于revolutionize）；使革命化
straightforward | adj. 简单的；坦率的；明确的；径直的
superfluous | adj. 多余的；不必要的；奢侈的

#### **【Comments and Conclusions】**

##### Compiler Support for C++20
　　https://en.cppreference.com/w/cpp/compiler_support gives you the answer to the core language and the library.  

##### Concepts
　　Concepts empower you to write requirements for your templates which can be checked by the compiler. Concepts revolutionise the way, we think about and write generic code. Here is why:  

　　- Requirements for templates are part of the interface.  
　　- The overloading of functions or specialisation of class templates can be based on concepts.  
　　- We get improved error message because the compiler compares the requirements of the template parameter with the actual template arguments.  

　　However, this is not the end of the story.  

　　- You can use predefined concepts or define your own.  
　　- The usage of auto and concepts is unified. Instead of auto, you can use a concept.  
　　- If a function declaration uses a concept, it automatically becomes a function template. Writing function templates is, therefore, as easy as writing a function.  

　　The following code snippet shows you the definition and the usage of the straightforward concept Integral:  
```c++
#include <concepts>
#include <iostream>
#include <type_traits>

template <typename T>
concept bool Integral() {
    return std::is_integral<T>::value;
}

Integral auto gcd(Integral auto a, Integral auto b) {
    if (b == 0) {
        return a;
    } else {
        return gcd(b, a % b);
    }
}

int main() {
    std::cout << gcd(10, 12) << std::endl;
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// 2
```
　　Integral is the concept which requires from it type-parameter T that std::is_integral<T>::value holds.If std::is_integral<T>::value evaluates to true, all is fine. If not, you get a compile-time error.   

　　gcd requires from its arguments and return type, that they support the concept Integral. gcd is a kind of function templates which puts requirements on its arguments and return value. When I remove the syntactic sugar, maybe you can see the real nature of gcd.   
```c++
#include <concepts>
#include <iostream>
#include <type_traits>

template <typename T>
concept bool Integral() {
    return std::is_integral<T>::value;
}

template <typename T>
requires(Integral<T>()) T gcd(T a, T b) {
    if (b == 0) {
        return a;
    } else {
        return gcd(b, a % b);
    }
}

int main() {
    std::cout << gcd(10, 12) << std::endl;
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// 2
```

##### Ranges Library
　　The ranges library is the first customer of concepts. It supports algorithms which  

　　- can operate directly on the container; you don't need iterators to specify a range  
　　- can be evaluated lazily  
　　- can be composed  

　　To make it short: The ranges library support functional patterns.   

　　The following functions show function composition with the pipe symbol.   
```c++
#include <iostream>
#include <ranges>
#include <vector>

int main() {
    std::vector<int> ints{0, 1, 2, 3, 4, 5};
    auto even = [](int i) { return 0 == i % 2; };
    auto square = [](int i) { return i * i; };

    for (int i : ints | std::views::filter(even) | std::views::transform(square)) {
        std::cout << i << ' ';
    }
    return 0;
}
// g++ -std=c++20 test.cpp -o test
// result:
// 0 4 16
```
　　even is a lambda function which returns if a i is even and the lambda function square maps i to its square.  

　　Apply on each element of ints the filter even and map each remaining element to its square.  

　　python implement compares c++ implement.  
```py
mylist = [i*i for i in range(0, 5) if i % 2 == 0]
print(mylist)
```

##### Coroutines
　　Coroutines are generalised functions that can be suspended and resumed while keeping their state. Coroutines are the usual way to write event-driven applications. An event-driven application can be simulations, games, servers, user interfaces, or even algorithms. Coroutines are also typically used for cooperative multitasking.  

　　Let me show you the usage of a special coroutine. The following program uses a generator for an infinite data-stream.  
```c++
#include <coroutine>
#include <iostream>
#include <vector>

/*
define Generator

template <typename T>
struct Generator {};
*/

Generator<int> getNext(int start = 0, int step = 1) {
    auto value = start;
    for (int i = 0;; ++i) {
        co_yield value;
        value += step;
    }
}

int main() {
    std::cout << "getNext():";
    auto gen = getNext();
    for (int i = 0; i <= 10; ++i) {
        gen.next();
        std::cout << " " << gen.getValue();
    }
    std::cout << std::endl;

    std::cout << "getNext(100, -10):";
    auto gen2 = getNext(100, -10);
    for (int i = 0; i <= 20; ++i) {
        gen2.next();
        std::cout << " " << gen2.getValue();
    }
    std::cout << std::endl;
    return 0;
}
// g++ -std=c++20 test.cpp -o test -fcoroutines
// result:
// getNext(): 0 1 2 3 4 5 6 7 8 9 10
// getNext(100, -10): 100 90 80 70 60 50 40 30 20 10 0 -10 -20 -30 -40 -50 -60 -70 -80 -90 -100
```

　　python implement compares c++ implement.  
```py
def getNext(start=0, step=1):
    value = start
    while True:
        yield value
        value = value + step


gen = getNext()
for i in range(11):
    print(next(gen))

gen2 = getNext(100, -10)
for i in range(21):
    print(next(gen2))
```

　　The function getNext is a coroutine because it uses the keyword co_yield. getNext has an infinite loop which returns the value after co_yield. A call to next() resumes the coroutine and the following getValue call gets the value. After the getNext call, the coroutine pauses once more. It pauses until the next next() call.   

##### Modules
　　For modules, I make it quite short because the post is already too long.  

　　Modules promise:  

　　- Faster compile times  
　　- Isolation of macros  
　　- Express the logical structure of the code  
　　- Make header files superfluous  
　　- Get rid of an ugly macro workarounds  

###  <font color=orange>tips: 批判性思维</font>
　　批判性思维包括：

　　1. 要能意识到它们是一整套环环相扣的关键问题。  
　　2. 有能力在适当时机以适当的方式提出和回答这些问题。  
　　3. 积极主动地使用这些关键问题的强烈渴望。  

###  <font color=orange>Share: 浅谈代理模式</font>

#### **【什么是代理模式？】**
　　《设计模式》一书中是这样定义的：为其他对象提供一种代理以控制对这个对象的访问。  

　　我们来看下代理模式的类图：  
![hello](/ARTS-34/1.jpg)

　　被代理类RealObject和代理类Proxy共同实现了Subject接口，这样代理类Proxy就可以替代实体。  

　　代理类Proxy会持有RealObject的引用，控制RealObject的Request等操作。  

#### **【代理模式的应用场景】**
　　1. 远程代理：RPC代理  

　　2. 虚代理：缓存实体的附加信息，以便延迟对它的访问  

　　3. 保护代理：控制对实体的访问权限  

　　4. 智能指引：c++中的智能指针  

　　5. 业务系统中非功能性需求：监控、统计、鉴权、限流、事务、幂等、日志  

#### **【代理模式的实现方法】**

#### 基于组合：
　　基于组合的方式就是上述的类图，代理类ImageProxy和原始类Image实现相同的接口Graphic，ImageProxy类通过委托的方式调用Image类中的方法。  

　　接着我们来看代码：  
```c++
#include <iostream>
#include <memory>
#include <vector>

struct Point {
    int x;
    int y;
};

class Graphic {
public:
    ~Graphic() { std::cout << "~Graphic()" << std::endl; }

    virtual void Draw(const Point& at) = 0;

protected:
    Graphic() { std::cout << "Graphic()" << std::endl; }
};

class Image : public Graphic {
public:
    Image(const std::string& file) { std::cout << "Image()" << std::endl; }
    ~Image() { std::cout << "~Image()" << std::endl; }

    virtual void Draw(const Point& at) { std::cout << "Draw() Point: " << at.x << "," << at.y << std::endl; }
};

class ImageProxy : public Graphic {
public:
    ImageProxy(const std::string& file) : file_(file) {
        std::cout << "ImageProxy()" << std::endl;
        std::cout << "file_: " << file_ << std::endl;
        extent_.x = 0;
        extent_.y = 0;
    }
    ~ImageProxy() { std::cout << "~ImageProxy()" << std::endl; }

    virtual void Draw(const Point& at) {
        std::cout << "ImageProxy Draw()" << std::endl;
        GetImage()->Draw(at);
    }

protected:
    std::shared_ptr<Image> GetImage() {
        if (!image_ptr_) {
            image_ptr_ = std::make_shared<Image>(file_);
        }
        return image_ptr_;
    }

private:
    Point extent_;
    std::string file_;
    std::shared_ptr<Image> image_ptr_;
};

class TextDocument {
public:
    TextDocument() { std::cout << "TextDocument()" << std::endl; }

    void Insert(const std::shared_ptr<Graphic>& graphic_ptr) { graphic_ptr_ = graphic_ptr; }

    void Draw(const Point& at) {
        std::shared_ptr<ImageProxy> proxy_ptr = std::dynamic_pointer_cast<ImageProxy>(graphic_ptr_);
        if (proxy_ptr) {
            proxy_ptr->Draw(at);
        } else {
            graphic_ptr_->Draw(at);
        }
    }

private:
    std::shared_ptr<Graphic> graphic_ptr_;
};

int main() {
    TextDocument document;
    document.Insert(std::make_shared<ImageProxy>("ImageFileName"));
    Point at;
    at.x = 1;
    at.y = 2;
    document.Draw(at);
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// TextDocument()
// Graphic()
// ImageProxy()
// file_: ImageFileName
// ImageProxy Draw()
// Graphic()
// Image()
// Draw() Point: 1,2
// ~ImageProxy()
// ~Image()
// ~Graphic()
// ~Graphic()
```

　　在ImageProxy类中，使用虚代理的方式，延迟对Image类的访问。  

　　在TextDocument类中，使用Insert()成员函数将Graphic指针增加进来，然后在Draw()函数中，对graphic_ptr_进行动态转型，如果graphic_ptr_指向ImageProxy，则调用代理类的Draw()函数，如果指向Image，则调用原始类的Draw()函数。  

#### 基于继承：
　　如果原始类没有定义接口，并且原始类代码无法修改，这个时候我们一般采用继承的方式。  

　　来看代码：  
```c++
#include <iostream>
#include <memory>
#include <vector>

struct Point {
    int x;
    int y;
};

class Image {
public:
    Image() { std::cout << "Image()" << std::endl; }
    ~Image() { std::cout << "~Image()" << std::endl; }

    virtual void Draw(const Point& at) { std::cout << "Draw() Point: " << at.x << "," << at.y << std::endl; }
};

class ImageProxy : public Image {
public:
    ImageProxy() : Image() { std::cout << "ImageProxy()" << std::endl; }
    ~ImageProxy() { std::cout << "~ImageProxy()" << std::endl; }

    virtual void Draw(const Point& at) {
        std::cout << "ImageProxy start Draw()" << std::endl;
        Image::Draw(at);
        std::cout << "ImageProxy end Draw()" << std::endl;
    }
};

int main() {
    ImageProxy proxy;
    Point at;
    at.x = 1;
    at.y = 2;
    proxy.Draw(at);
    return 0;
}
// g++ -std=c++11 test.cpp -o test
// result:
// Image()
// ImageProxy()
// ImageProxy start Draw()
// Draw() Point: 1,2
// ImageProxy end Draw()
// ~ImageProxy()
// ~Image()
```

　　代理类ImageProxy继承原始类Image，调用原始的函数，并扩展附加功能。在代码我们通过打印开始与结束日志等功能附加在Image::Draw()前后。  

#### Java中的动态代理：
　　Java中的动态代理有两种方式：  

　　第一种是JDK动态代理，是通过接口的方式。  
```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

interface Graphic {
    void print();
}

class Image implements Graphic {
    public void print() {
        System.out.println("print Image()");
    }
}

public class ImageProxy implements InvocationHandler {
    private Object proxiedObject;

    ImageProxy(Object proxiedObject) {
        this.proxiedObject = proxiedObject;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        before();
        Object result = method.invoke(proxiedObject, args);
        after();
        return result;
    }

    public void before() {
        System.out.println("before()");
    }

    public void after() {
        System.out.println("after()");
    }

    public static void main(String[] args) {
        Image image = new Image();
        ImageProxy proxy = new ImageProxy(image);
        Graphic graphicProxy = (Graphic) Proxy.newProxyInstance(image.getClass().getClassLoader(), image.getClass().getInterfaces(), proxy);
        graphicProxy.print();
    }
}
// before()
// print Image()
// after()
```

　　ImageProxy作为动态代理类实现InvocationHandler接口，其中在InvocationHandler.invoke方法中，实现被代理对象调用方法时期望执行的动作。在这段代码中，我们希望在被代理对象Image调用方法前后打印log。  

　　Proxy.newProxyInstance方法通过代理类和被代理类来构造代理类的新实例，所有被代理类的方法会触发invoke方法。  


　　第二种是CGLIB动态代理，是通过继承方式。  
```java
import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

class Image {
    public void print() {
        System.out.println("print Image()");
    }
}

public class ImageProxy implements MethodInterceptor {
    private Object proxiedObject;

    public Object createProxy(Object target) {
        this.proxiedObject = target;
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(this.proxiedObject.getClass());
        enhancer.setCallback(this);
        return enhancer.create();
    }

    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        before();
        Object object = methodProxy.invokeSuper(o, objects);
        after();
        return object;
    }

    public void before() {
        System.out.println("before()");
    }

    public void after() {
        System.out.println("after()");
    }

    public static void main(String[] args) {
        Image image = new Image();
        ImageProxy imageProxy = new ImageProxy();
        Image proxy = (Image) imageProxy.createProxy(image);
        proxy.print();
    }
}
// before()
// print Image()
// after()
```

　　Image类并不需要实现任何接口，CGLIB采用继承的方式来实现动态代理。ImageProxy.createProxy方法中Enhancer类将Image类设置为父类。  

　　当被代理对象Image调用方法时，会进行拦截，在方法前后打印log。  