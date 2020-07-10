---
layout: post
title: ARST(六)
date: 2020-05-25 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=orange>Algorithm</font>

#### **【LeetCode:20. Valid Parentheses】**

##### 　　Description:
　　Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.  

　　An input string is valid if:  
　　　　1.Open brackets must be closed by the same type of brackets.  
　　　　2.Open brackets must be closed in the correct order.  

　　Note that an empty string is also considered valid.  


##### 　　Example 1:
```sh
Input: "()"
Output: true
```

##### 　　Example 2:
```sh
Input: "()[]{}"
Output: true
```

##### 　　Example 3:
```sh
Input: "(]"
Output: false
```

##### 　　Example 4:
```sh
Input: "([)]"
Output: false
```

##### 　　Example 5:
```sh
Input: "{[]}"
Output: true
```

##### 　　C++ Solution:
```cpp
/*
解题思路
a. 左括号 -> push
b. 右括号 ->
        b.1 stack empty: return false
        b.2 stack not empty ->
                                b.2.1 peek: if match, pop
                                b.2.2 peek: not match, return false
c. 扫描完   stack empty: return true
            stack not empty: return false

时间复杂度: O(n)

*/

#include <assert.h>

#include <iostream>
#include <stack>
#include <string>

class Solution {
public:
    bool isValid(std::string s) {
        std::stack<char> stack;
        for (auto c : s) {
            if (isLeftBracket(c)) {
                stack.push(c);
            } else {
                if (stack.empty()) {
                    return false;
                } else {
                    auto top = stack.top();
                    if (!isMatch(top, c)) {
                        return false;
                    } else {
                        stack.pop();
                    }
                }
            }
        }
        return stack.empty();
    }

private:
    bool isLeftBracket(char c) {
        switch (c) {
            case '(':
            case '{':
            case '[':
                return true;
                break;
            default:
                return false;
                break;
        }
        return false;
    }

    bool isMatch(char left, char right) {
        switch (left) {
            case '(':
                return right == ')';
                break;
            case '{':
                return right == '}';
                break;
            case '[':
                return right == ']';
                break;
            default:
                break;
        }
        return false;
    }
};
```

###  <font color=orange>Review</font>

#### **【Article】**
http://www.aosabook.org/en/distsys.html (1.3. The Building Blocks of Fast and Scalable Data Access[Load Balancers, Queues])  

#### **【Words】**

English | Chinese
-|-
criteria | n. 标准，条件
sophisticated  | adj. 复杂的；精致的；久经世故的；富有经验的


#### **【Comments and Conclusions】**
　　Load Balancers chart:  
![hello](/ARTS-6/1.png)  
　　1.Load balancer  

![hello](/ARTS-6/2.png)  
　　2.Multiple load balancers  

　　Load Balancers chart:  
![hello](/ARTS-6/3.png)  
　　1.Synchronous request  

![hello](/ARTS-6/4.png)  
　　2.Using queues to manage requests  

###  <font color=orange>tips：高效阅读的秘密</font>
　　寻找三个闪光点：  
　　读书时要一边输入（读书），一边有意识的输出（讲诉）。  
　　在书的前半部分，中间部分和后半部分各选择一个闪光点，在平衡全书的基础上实现更好的讲诉。  

###  <font color=orange>Share：boost::bind库学习笔记</font>

#### **【简介】**
　　bind是c++98标准库函数适配器bind1st/bind2nd的泛化和增强，可以适配任意的可调用对象，包括函数指针、函数引用、成员函数指针和函数对象。  

#### **【工作原理】**
　　bind接受的第一个参数是一个可调用对象f，之后bind接受最多九个参数，参数的数量必须和f的参数数量相等，这些参数将被传递给f作为入参。  
　　绑定完成后，bind会返回一个函数对象，它内部保存了f的拷贝，具有operator()，返回值类型被自动推导为f的返回值类型。在发生调用时，这个函数对象将把之前存储的参数转发给f完成调用。  
　　例如，有一个函数f，它的形式是：  
```c++
f(a1, a2)
```
　　那么它将等价于一个具有无参operator()的bind函数对象调用：  
```c++
bind(f, a1, a2)()
```
　　bind表达式存储了f和a1，a2的拷贝，产生了一个临时函数对象。因为f接受两个参数，而a1和a2都是实参，因此临时函数对象将具有一个无参数的operator()。当operator()调用发生时函数对象把a1、a2的拷贝传递给f，完成真正的函数调用。  
　　bind的最强大之处在于它的占位符，被定义为_1、_2、_3一直到_9，位于一个匿名的名字空间。占位符可以取代bind中参数的位置，在发生函数调用时才接受真正的参数。  
　　占位符的名字表示它在调用式中的顺序，而在绑定表达式中没有顺序的要求，_1不一定必须第一个出现，也不一定只出现一次，例如：  
```c++
bind(func, _2, _1)(a1, a2)
```
　　返回一个具有两个参数的函数对象，第一个参数将放置函数func的第二个位置，第二个参数将放置函数func的第一个位置，调用时等价于  
```c++
func(a2, a1)
```

#### **【绑定成员函数】**
　　绑定普通函数没啥好说的，下面来着重介绍绑定成员函数。  
　　类的成员函数不同于普通函数，因为成员函数指针不能直接调用operator()，它必须被绑定到一个对象(引用、指针)上，然后才能得到this指针进而调用成员函数。因此bind需要牺牲一个占位符的位置，即：  
```c++
bind(&X::func, x, _1, _2, ...)
```
　　这意味着使用成员函数最多绑定八个参数。  
　　例如：  
```c++
struct A{
    int f(int a, int b){
        return a + b;
    }
}；
```
　　那么下面的bind的表达式都是成立的：  
```c++
A a;
A &ra = a;
A *pa = &a;

cout << bind(&A::f, a, _1, 20)(10) << endl;
cout << bind(&A::f, ra, _2, _1)(10, 20) << endl;
cout << bind(&A::f, p, _1, _2)(10, 20) << endl;
```

#### **【绑定函数对象】**
　　bind不仅能够绑定普通函数和成员函数，也能够绑定任意的函数对象，包括标准库中的所有预定义的函数对象。  
　　如果函数对象有内部类型定义result_type，那么bind可以自动推导出返回值类型，用法与绑定普通函数一样。但如果函数对象没有定义result_type，则需要在绑定形式上做一点改动，用模板参数指明返回类型：  
```c++
bind<result_type>(Functor, ...)
```
　　标准库和Boost库中的大部分函数对象都具有result_type定义，因此不需要特别的形式就可以直接使用bind，例如：  
```c++
bind(std::greater<int>(), _1, 10);   //检查x>10
bind(plus<int>(), _1, _2);           //执行x+y
bind(modulus<int>(), _1, 3);         //执行x%3
```
　　对于自定义的函数对象，如果没有定义result_type类型定义，例如：  
```c++
struct f{
    int operator()(int a, int b){
        return a + b;
    }
};
```
　　那么我们必须指明bind的返回值类型，像这样：
```c++
cout << bind<int>(f(), _1, _2)(10, 20) << endl;
```
　　这种写法多少有些不便，因此，在编写自己的函数对象时，最好遵循规范为它们增加内部result_type。  

#### **【项目应用】**
　　项目中有个rmodule.h头文件，定义回调如下：  
```c++
/** Enum callback */
typedef boost::function<void (RRepository &)> RModuleEnumCallback;
/** Wrap callback */
typedef boost::function<void (RInstancesWrapper &)> RModuleWrapCallback;

typedef struct _callbacks_t {
    RModuleEnumCallback m_enum_callback;
    RModuleWrapCallback m_wrap_callback;
} callbacks_t;

std::map<std::string, callbacks_t> m_callbacks;
```
　　\_callbacks_t中有两个回调函数，被boost::function函数对象的“容器”封装。boost::function能够容纳任意符合函数签名的可调用对象。  
　　因此它可以被用于回调机制，暂时保管函数或函数对象，在之后需要的时机再调用，使回调机制拥有更多的弹性。  
　　项目中RNetworkModule类继承于RModule类，是这样初始化回调函数的：  
```c++
callbacks_t cb;

cb.m_enum_callback = boost::bind(&RNetworkModule::EnumAdapterInstances, this, _1);
cb.m_wrap_callback = boost::bind(&RNetworkModule::WrapAdapterInstances, this, _1);
m_callbacks[T_NETWORK_ADAPTER] = cb;
```
　　boost::bind绑定的是一个成员函数，第二个参数是this指针。  
　　最后执行回调函数：  
```c++
void RModule::EnumInstances(const string &type, RRepository &repo) {
    if (!m_init) {
        InitCallbacks();
        m_init = true;
    }
    map<string, callbacks_t>::const_iterator it = m_callbacks.find(type);
    if (it == m_callbacks.end()) {
        oWarn() << "Failed to find enum callback in " << GetName() << " for type: " << type << endll;
    } else {
        oInfo() << "Begin to enum instances for type: " << type << endll;
        it->second.m_enum_callback(repo);
    }
}
```
