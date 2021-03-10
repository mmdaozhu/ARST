---
layout: post
title: ARST(十九)
date: 2020-08-24 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=blue>Algorithm</font>

#### **【LeetCode:200. Number of Islands】**
　　Given an m x n 2d grid map of '1's (land) and '0's (water), return the number of islands.  
　　An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.  

##### 　　Example 1:
```sh
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

##### 　　Example 2:
```sh
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

##### 　　C++ Solution 1:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/200.NumberofIslands/NumberofIslands1.cpp  

##### 　　C++ Solution 2:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/200.NumberofIslands/NumberofIslands2.cpp  

###  <font color=blue>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
incur | v. 招致，遭受；引致，带来

#### **【Comments and Conclusions】**

##### Content delivery network
　　A content delivery network (CDN) is a globally distributed network of proxy servers, serving content from locations closer to the user. Generally, static files such as HTML/CSS/JS, photos, and videos are served from CDN, although some CDNs such as Amazon's CloudFront support dynamic content. The site's DNS resolution will tell clients which server to contact.  

　　Serving content from CDNs can significantly improve performance in two ways:  
　　- Users receive content from data centers close to them  
　　- Your servers do not have to serve requests that the CDN fulfills  

##### Push CDNs
　　Push CDNs receive new content whenever changes occur on your server. You take full responsibility for providing content, uploading directly to the CDN and rewriting URLs to point to the CDN. You can configure when content expires and when it is updated. Content is uploaded only when it is new or changed, minimizing traffic, but maximizing storage.  

　　Sites with a small amount of traffic or sites with content that isn't often updated work well with push CDNs. Content is placed on the CDNs once, instead of being re-pulled at regular intervals.  

##### Pull CDNs
　　Pull CDNs grab new content from your server when the first user requests the content. You leave the content on your server and rewrite URLs to point to the CDN. This results in a slower request until the content is cached on the CDN.  

　　A time-to-live (TTL) determines how long content is cached. Pull CDNs minimize storage space on the CDN, but can create redundant traffic if files expire and are pulled before they have actually changed.  

　　Sites with heavy traffic work well with pull CDNs, as traffic is spread out more evenly with only recently-requested content remaining on the CDN.  

##### Disadvantage(s): CDN
　　- CDN costs could be significant depending on traffic, although this should be weighed with additional costs you would incur not using a CDN.  
　　- Content might be stale if it is updated before the TTL expires it.  
　　- CDNs require changing URLs for static content to point to the CDN.  
　
###  <font color=blue>tips: 前Facebook面试官覃超揭秘他学习算法与数据结构的历程</font>

https://www.bilibili.com/video/BV1gJ411b7zV  

　　1.视频：1.5-2.0倍数播放，暂停+难点反复  
　　　　- 三分理解、三分练习  
　　　　- 快速的实现业务（效率）  

　　2.大道至简、思考本质  
　　　　- 最近的重复性->复杂问题的内在规律  
　　　　- 短时间、少代码解决  

　　3.摒弃”旧习惯  
　　　　- 不要死磕  
　　　　- 五毒神掌（敢于放手）  
　　　　- 不懒于看高手代码（国际版的高票回答）  

　　脑图  

###  <font color=blue>Share: boost.assign库下的职责链模式</font>
　　职责链模式：将请求的发送和接收解耦，让多个接收对象都有机会处理这个请求。将这些接收对象串成一条链，并沿着这条链传递这个请求，直到链上的某个接收对象能够处理它为止。  

　　在职责链模式中，多个处理器依次处理同一个请求。一个请求先经过 A 处理器处理，然后再把请求传递给 B 处理器，B 处理器处理完后再传递给 C 处理器，以此类推，形成一个链条。链条上的每个处理器各自承担各自的处理职责，所以叫作职责链模式。  

　　在 GoF 的定义中，一旦某个处理器能处理这个请求，就不会继续将请求传递给后续的处理器了。当然，在实际的开发中，也存在对这个模式的变体，那就是请求不会中途终止传递，而是会被所有的处理器都处理一遍。  

#### **【boost.assign库简介】**
　　boost.assign库的工作原理类似职责链模式，但是链上仅有一个对象，它使用重载操作符将赋值请求连接成一个链逐个处理，最后完成初始化和赋值操作。  

#### **【使用操作符+=向容器增加元素】**
　　使用assign库时必须使用using指示符，只有这样才能让重载的+=,等操作符在作用域内生效。  

　　代码示例：  
```c++
#include <boost/assign.hpp>
#include <iostream>
#include <set>
#include <string>
#include <vector>

int main() {
    using namespace boost::assign;
    std::vector<int> v;
    v += 1, 2, 3, 4, 5, 6 * 6;
    for (auto& e : v) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::set<std::string> s;
    s += "cpp", "java", "c#", "python";
    for (auto& e : s) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::map<int, std::string> m;
    m += std::make_pair(1, "one"), std::make_pair(2, "two");
    for (auto& e : m) {
        std::cout << e.first << "->" << e.second << ",";
    }
    std::cout << std::endl;

    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 1,2,3,4,5,36,
// c#,cpp,java,python,
// 1->one,2->two,
```
　　operator+=很好用，但是有点遗憾，它仅限应用于STL中定义的标准容器(vector、list、set等)。  

#### **【使用操作符()向容器增加元素】**
　　我们不能直接使用operator()，而应当使用assign库提供的四个辅助函数insert()、push_font()、push_back()、push()。这些函数可作用于拥有同名成员函数的容器，接受容器变量作为参数，返回一个代理对象list_inserter，它重载了operator(),=等操作符用来实现向容器增加元素的功能。  

　　代码示例：  
```c++
#include <boost/assign.hpp>
#include <iostream>
#include <list>
#include <set>
#include <stack>
#include <string>
#include <vector>
#include <queue>

int main() {
    using namespace boost::assign;
    std::vector<int> v;
    push_back(v)(1)(2)(3)(4)(5), 6, 7, 8, (9), 10;
    for (auto& e : v) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::list<std::string> l;
    push_front(l)("cpp")("java")("c#")("python");
    for (auto& e : l) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::set<double> s;
    insert(s)(3.14)(0.618)(1.732);
    for (auto& e : s) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::map<int, std::string> m;
    insert(m)(1, "one")(2, "two");
    for (auto& e : m) {
        std::cout << e.first << "->" << e.second << ",";
    }
    std::cout << std::endl;

    std::stack<int> stk;
    push(stk)(1)(2)(3)(4);
    while(!stk.empty()){
        std::cout << stk.top() << ",";
        stk.pop();
    }
    std::cout << std::endl;

    std::queue<int> que;
    push(que)(1)(2)(3)(4);
    while(!que.empty()){
        std::cout << que.front() << ",";
        que.pop();
    }
    std::cout << std::endl;    

    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 1,2,3,4,5,6,7,8,9,10,
// python,c#,java,cpp,
// 0.618,1.732,3.14,
// 1->one,2->two,
// 4,3,2,1,
// 1,2,3,4,
```
　　operator()的好处是可以在括号中使用多个参数，对于map这样的元素是由多个值组成的类型非常方便，避免了使用make_pair()。  

　　不过括号运算符也可以和逗号运算符配合使用。  

#### **【初始化容器元素】**
　　assign库使用list_of()、map_list_of()和tuple_list_of()三个函数进行初始化容器。  

　　代码示例：  
```c++
#include <boost/assign.hpp>
#include <iostream>
#include <list>
#include <set>
#include <string>
#include <vector>

int main() {
    using namespace boost::assign;
    std::vector<int> v = list_of(1)(2)(3)(4)(5);
    for (auto& e : v) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::list<std::string> l = (list_of("cpp")("java"), "c#", "python");
    for (auto& e : l) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::set<double> s = (list_of(3.14), 0.618, 1.732);
    for (auto& e : s) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::map<int, std::string> m = map_list_of(1, "one")(2, "two");
    for (auto& e : m) {
        std::cout << e.first << "->" << e.second << ",";
    }
    std::cout << std::endl;

    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 1,2,3,4,5,
// cpp,java,c#,python,
// 0.618,1.732,3.14,
// 1->one,2->two,
```

#### **【解决重复输入】**
　　assign库提供repeat()、repeat_fun()和range()三个函数来解决重复输入问题。  

　　我们来看下这三个函数的定义：  
```c++
    template< class Function, class Argument = assign_detail::forward_n_arguments > 
    class list_inserter {
        template< class T >
        list_inserter& repeat( std::size_t sz, T r )
        {
            std::size_t i = 0;
            while( i++ != sz )
                insert_( r );
            return *this;
        }
        
        template< class Nullary_function >
        list_inserter& repeat_fun( std::size_t sz, Nullary_function fun )
        {
            std::size_t i = 0;
            while( i++ != sz )
                insert_( fun() );
            return *this;
        }

        template< class SinglePassIterator >
        list_inserter& range( SinglePassIterator first, 
                              SinglePassIterator last )
        {
            for( ; first != last; ++first )
                insert_( *first );
            return *this;
        }
    };
```

　　repeat()函数第一个参数指定重复的次数，第二个参数是需要重复的元素；repeat_fun()函数同样第一个参数是重复的次数，不过第二个参数是个函数对象，返回增加的元素；range()函数可以把一个序列的全部或部分插入到另一个序列中。  
　　代码示例：  
```c++
#include <boost/assign.hpp>
#include <cstdlib>
#include <deque>
#include <iostream>
#include <set>
#include <string>
#include <vector>

int main() {
    using namespace boost::assign;
    std::vector<int> v = list_of(1).repeat(4, 5);
    for (auto& e : v) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::set<int> s;
    insert(s).repeat_fun(5, &rand);
    for (auto& e : s) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    std::deque<int> d;
    push_front(d).range(v.begin(), v.end());
    for (auto& e : d) {
        std::cout << e << ",";
    }
    std::cout << std::endl;

    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 1,5,5,5,5,
// 846930886,1681692777,1714636915,1804289383,1957747793,
// 5,5,5,5,1,
```
