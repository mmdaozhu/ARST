---
layout: post
title: ARST(十三)
date: 2020-07-13 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=green>Algorithm</font>

#### **【LeetCode:32. Longest Valid Parentheses】**

##### 　　Description:
　　Given an input string, reverse the string word by word.  

##### 　　Example 1:
```sh
Input: "the sky is blue"
Output: "blue is sky the"
```

##### 　　Example 2:
```sh
Input: "  hello world!  "
Output: "world! hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

##### 　　Example 3:
```sh
Input: "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

##### 　　Note
　　A word is defined as a sequence of non-space characters.  
　　Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.  
　　You need to reduce multiple spaces between two words to a single space in the reversed string.  

##### 　　C++ Solution:
```cpp
#include <assert.h>

#include <iostream>
#include <string>

/*
解体思路：
     ****the*sky**is*blue**
     1.**eulb*si**yks*eht**** (swap)
     2.**eulb*si**yks*eht**** (swap,[0,6))
     3.blue***si**yks*eht**** (swap,[5,9))
     4.blue*is****yks*eht**** (swap,[8,14))
     5.blue*is*sky****eht**** (swap,[12,18))
     6.blue*is*sky*the******* (remove *)
     7.blue*is*sky*the

时间复杂度分析：O(n)
*/

class Solution {
public:
    std::string reverseWords(std::string s) {
        for (size_t i = 0; i < s.size() / 2; i++) {
            std::swap(s[i], s[s.size() - i - 1]);
        }
        int start(0);
        int end(-2);
        bool inWord(false);
        for (size_t i = 0; i <= s.size(); i++) {
            if (i != s.size() && s[i] != ' ') {
                inWord = true;
            }
            if (inWord && (i == s.size() || s[i] == ' ')) {
                inWord = false;
                start = end;
                while (start > 0 && s[start] == ' ') {
                    start--;
                }
                start += 2;
                end = i;
                for (int k = 0; k < (end - start) / 2; k++) {
                    std::swap(s[k + start], s[end - k - 1]);
                }
            }
        }
        while (!s.empty() && s.back() == ' ') {
            s.pop_back();
        }
        return s;
    }
};
```

###  <font color=green>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  
https://community.cadence.com/cadence_blogs_8/b/sd/posts/understanding-latency-vs-throughput  
https://www.slideshare.net/jboner/scalability-availability-stability-patterns/  

#### **【Words】**
English | Chinese
-|-
versus | prep. 对，对抗；与……相对，与……相比


#### **【Comments and Conclusions】**
　　Latency is the time required to perform some action or to produce some result.  
　　Throughput is the number of such actions executed or results produced per unit of time.  

###  tips

###  <font color=green>Share: boost库下的桥接模式</font>
　　首先了解一下桥接模式。  
　　桥接模式是这么定义的，将抽象和实现解耦，让它们可以独立变化。  
　　还有另外一种理解方式：“一个类存在两个（或多个）独立变化的维度，我们通过组合的方式，让这两个（或多个）维度可以独立进行扩展。“  

#### **【智能指针的pimpl用法】**
　　在c++具体的编程实践中，桥接模式也被称为pimpl，它可以将头文件的依赖关系降到最小，减少编译时间，而且可以不使用虚函数实现多态。  
```c++
#include <boost/shared_ptr.hpp>
#include <iostream>
#include <vector>

class Impl {
public:
    Impl() {}
    void print() { std::cout << "impl print" << std::endl; }
};

class A {
public:
    A() : pImpl(new Impl) {}
    void print() { pImpl->print(); }

private:
    boost::shared_ptr<Impl> pImpl;
};

int main() {
    A a;
    a.print();
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// impl print
```

#### **【random_device的pimpl用法】**
　　我们先看下random_device的class声明。  
```c++
class random_device : private noncopyable
{
public:
    typedef unsigned int result_type;
    BOOST_STATIC_CONSTANT(bool, has_fixed_range = false);

    /** Returns the smallest value that the \random_device can produce. */
    static result_type min BOOST_PREVENT_MACRO_SUBSTITUTION () { return 0; }
    /** Returns the largest value that the \random_device can produce. */
    static result_type max BOOST_PREVENT_MACRO_SUBSTITUTION () { return ~0u; }

    /** Constructs a @c random_device, optionally using the default device. */
    BOOST_RANDOM_DECL random_device();
    /** 
     * Constructs a @c random_device, optionally using the given token as an
     * access specification (for example, a URL) to some implementation-defined
     * service for monitoring a stochastic process. 
     */
    BOOST_RANDOM_DECL explicit random_device(const std::string& token);

    BOOST_RANDOM_DECL ~random_device();

    /**
     * Returns: An entropy estimate for the random numbers returned by
     * operator(), in the range min() to log2( max()+1). A deterministic
     * random number generator (e.g. a pseudo-random number engine)
     * has entropy 0.
     *
     * Throws: Nothing.
     */
    BOOST_RANDOM_DECL double entropy() const;
    /** Returns a random value in the range [min, max]. */
    BOOST_RANDOM_DECL unsigned int operator()();

    /** Fills a range with random 32-bit values. */
    template<class Iter>
    void generate(Iter begin, Iter end)
    {
        for(; begin != end; ++begin) {
            *begin = (*this)();
        }
    }

private:
    class impl;
    impl * pimpl;
};
```
　　random_device定义了必要的接口，真正的实现是impl。  
　　接下来我们来看看random_device的实现。  
```c++
BOOST_RANDOM_DECL boost::random::random_device::random_device()
  : pimpl(new impl(default_token))
{}

BOOST_RANDOM_DECL boost::random::random_device::random_device(const std::string& token)
  : pimpl(new impl(token))
{}

BOOST_RANDOM_DECL boost::random_device::~random_device()
{
  delete pimpl;
}

BOOST_RANDOM_DECL double boost::random_device::entropy() const
{
  return 10;
}

BOOST_RANDOM_DECL unsigned int boost::random_device::operator()()
{
  return pimpl->next();
}
```
　　random_device的接口其实就是pimpl实现的。  
　　后面我们来看下pimpl的linux实现。  
```c++
namespace {
// the default is the unlimited capacity device, using some secure hash
// try "/dev/random" for blocking when the entropy pool has drained
const char * const default_token = "/dev/urandom";
}

class boost::random::random_device::impl
{
public:
  impl(const std::string & token) : path(token) {
    fd = open(token.c_str(), O_RDONLY);
    if(fd < 0)
      error("cannot open");
  }

  ~impl() { if(close(fd) < 0) error("could not close"); }

  unsigned int next() {
    unsigned int result;
    std::size_t offset = 0;
    do {
      long sz = read(fd, reinterpret_cast<char *>(&result) + offset, sizeof(result) - offset);
      if(sz == -1)
        error("error while reading");
      else if(sz == 0) {
        errno = 0;
        error("EOF while reading");
      }
      offset += sz;
    } while(offset < sizeof(result));
    return result;
  }

private:
  void error(const char * msg) {
    int error_code = errno;
    boost::throw_exception(
      boost::system::system_error(
        error_code, boost::system::system_category(),
        std::string("boost::random_device: ") + msg + 
        " random-number pseudo-device " + path));
  }
  const std::string path;
  int fd;
};
```
　　在linux环境下random_device默认使用/dev/urandom作为token产生随机数。  

#### **【random_device的使用】**
```c++
#include <boost/nondet_random.hpp>
#include <iostream>
#include <vector>

int main() {
    boost::random_device rng;
    for (auto i = 0; i < 10; ++i) {
        std::cout << rng() << ",";
    }
    std::cout << std::endl;
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test -L/var/boost_1_73_0/lib/ -lboost_random
// result:
// 3167130624,3508750022,192526615,104753998,590374395,1475663833,3621627913,3981076437,1615288177,2037227441,
```