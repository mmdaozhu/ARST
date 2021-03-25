---
layout: post
title: ARST(二十一)
date: 2020-09-07 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=indigo>Algorithm</font>

#### **【LeetCode:64. Minimum Path Sum】**

##### 　　Description:
　　Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.  

　　Note: You can only move either down or right at any point in time.  

##### 　　Example 1:
```sh
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```

##### 　　Example 2:
```sh
Input: grid = [[1,2,3],[4,5,6]]
Output: 12
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/064.MinimumPathSum/MinimumPathSum.cpp  

###  <font color=indigo>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
centralize | vt. 使集中；使成为…的中心

#### **【Comments and Conclusions】**

##### Reverse proxy (web server)
　　A reverse proxy is a web server that centralizes internal services and provides unified interfaces to the public. Requests from clients are forwarded to a server that can fulfill it before the reverse proxy returns the server's response to the client.  

　　Additional benefits include:  

　　- Increased security - Hide information about backend servers, blacklist IPs, limit number of connections per client  
　　- Increased scalability and flexibility - Clients only see the reverse proxy's IP, allowing you to scale servers or change their configuration  
　　- SSL termination - Decrypt incoming requests and encrypt server responses so backend servers do not have to perform these potentially expensive operations  
　　- Compression - Compress server responses  
　　- Caching - Return the response for cached requests  
　　- Static content - Serve static content directly  

##### Load balancer vs reverse proxy
　　- Deploying a load balancer is useful when you have multiple servers. Often, load balancers route traffic to a set of servers serving the same function.  
　　- Reverse proxies can be useful even with just one web server or application server, opening up the benefits described in the previous section.  
　　- Solutions such as NGINX and HAProxy can support both layer 7 reverse proxying and load balancing.  

##### Disadvantage(s): reverse proxy
　　Introducing a reverse proxy results in increased complexity.  
　　A single reverse proxy is a single point of failure, configuring multiple reverse proxies (ie a failover) further increases complexity.  
　
###  <font color=indigo>tips: 惊呆！Python 竟然还有这样的黑魔法！</font>

https://www.bilibili.com/video/BV1FJ411J7CR  

##### 1. 基本数据类型
　　1.推导式  

　　推导式：构建列表、字典、集合和生成器便捷方式  
　　推导式语法：[表达式 for 迭代变量 in 可迭代对象 if 条件]  

　　列表推导式：  
```py
mylist = [i**2 for i in range(1, 11) if i > 5]
print(mylist)
```

　　字典推导式：  
```py
mydict = {i: i*i for i in (5, 6, 7)}
print(mydict)
```

　　集合推导式：  
```py
myset = {i for i in 'HarryPotter' if i not in 'er'}
print(myset)
```

　　元组推导式：  
```py
mytuple = tuple(i for i in range(0, 11))
print(mytuple)
```

　　生成器推导式：  
```py
mygenerator = (i for i in range(0, 11))
print(mygenerator)
print(list(mygenerator))
```

　　循环推导式：  
```py
myloop = [i+j for i in range(0, 3) for j in range(0, 2)]
print(myloop)
```

　　字典转换列表：  
```py
mydict = {'1' : 'a', '2' : 'b'}
mylist = [key + ':' + value for key, value in mydict.items()]
print(mylist)
```

　　2.字符串的连接和拆分  

　　常用连接方法：  
```py
mylist = ['time', 'geekbang', 'org']
print(''.join(mylist))
```

　　拆分方式：  
```py
mystring = 'time.geekbang.org'
print(mystring.split('.')[0])

var1, var2 = mystring.split('.')[1:3]
print(var1)
print(var2)
```

　　3.格式化字符串  

　　%操作符  
```py
import math
print('The value of Pi is approximately %5.3f.' % math.pi)
```

　　.format -- 更加灵活：  
```py
print('{1} and {0}'.format('spam', 'eggs'))
print('The story of {0}, {1} and {other}.'.format('Bill', 'Manfred', other='Georg'))
```

　　fstring -- 更加强大：  
```py
firstname = 'yin'
secondname = 'wilson'
print(f'Hello, {secondname} {firstname}')
print(f'{ 2 * 5}')

class Person:
    def __init__(self, first_name, second_name):
        self.first_name = first_name
        self.second_name = second_name

    def __str__(self):
        return f'hello, {self.first_name} {self.second_name}.'

me = Person('yin', 'wilson')
print(f'{me}')
```

　　4.使用collections扩展内置数据类型  
　　namedtuple -- 带命名的元组  
```py
import collections

Point = collections.namedtuple('Point', ['x', 'y'])
p = Point(11, y=22)
print(p[0] + p[1])
print(p.x + p.y)
print(p)
```
　　deque  
　　counter  

##### 2. 函数

　　1.函数的可变长参数  
　　定义如下：  
```py
def func(*args, **kargs):
    pass
```
　　示例：  
```py
def func(*args, **kargs):
    print(f'args: {args}')
    print(f'kargs: {kargs}')

func(123, 'xyz', name='xvalue')
```

　　2.Lambad表达式  
```py
k = lambda x:x+1
print(k(1))
```

　　3.高阶函数  
　　高阶：参数是函数、返回值是函数  
　　常见的高阶函数：map、reduce、filter  
　　推导式和生成器表达式可以替代map和filter函数  

　　map(函数，序列)将序列中每个值传入函数，处理完成返回map对象  
```py
number = list(range(11))

def square(x):
    return x**2

print(list(map(square, number)))
print(dir(map(square, number)))
```

　　filter(函数，序列)将序列中每个值传入函数，符合函数条件的返回filter对象  

　　4.生成器  
　　- 在函数中使用yield关键字，可以实现生成器  
　　- 生成器可以让函数返回可迭代对象  
　　- yield和return不同，return返回后，函数状态终止，yield保持函数的执行状态，返回后，函数回到之前保存的状态继续执行  
　　- 函数被yield暂停，局部变量也会被保存  
　　- 迭代器终止时，会抛出StopIteration异常  
```py
gennumber = (i for i in range(0,11))
print(next(gennumber))
print(next(gennumber))
print(next(gennumber))
print(next(gennumber))
print([i for i in gennumber])
```

　　5.装饰器  
　　- 装饰器函数的参数是被装饰函数  
　　- 装饰器函数处理被装饰函数，处理完成后返回被装饰函数或者替换为另一个函数  
　　- 装饰器语法糖的展开  
```py
@decorate
def target():
    print('do something')

def target():
    print('do something')
target = decorate(target)
```
　　- 装饰器不用入侵函数内部  
　　- 装饰器强调定义态，而非运行态  
```py
def decorate(func):
    def inner():
        print('in decorator')
        func()
    return inner

@decorate
def target():
    print('do something')

target()
```

##### 3. 面向对象
　　1.魔术方法  
　　- 魔术方法以2个下划线开头，2个下划线结束  
　　- 魔术方法类似其他语言的接口  
　　- 自定义类实现自带的数据类型  
　　- 如：  
　　　　- \__call__()允许类的实例成为可调用对象  
　　　　- \__str__()被str()调用时的行为  
　　　　- \__getitem__()使用[key]调用时的行为  
　　　　- \__iter__()被迭代时的行为  
　　- 鸭子类型  

　　2.属性描述符property  
　　- 描述符：实现特定协议的类  
　　- property类需要实现\__get__、\__set__、\__delete__方法  
```py
class Teacher:
    # def __init__(self, name):
    #     self.name = name
    def __get__(self, name):
        return self.name
    def __set__(self, name):
        self.name = name

teacher = Teacher()
teacher.name = 'wilson'
print(teacher.name)
```

　　3.抽象基类  
　　- 抽象基类提供了逻辑和实现解耦的能力  
　　- 父类可以定义，不用实现  
　　- 子类必须实现，否则报错  

　　4.ORM  
```py
pip install SQLAlchemy
```

##### 4 程序健壮性
　　1.运行性能  
　　- dis模板可以对Python代码进行生成字节码操作  
```py
import dis

def func():
    a = list('123')
    b = ['123']

print(dis.dis(func))
```

　　2.异常捕获  
　　- 所有内置的非系统退出的异常都派生自Exception类  
```py
gennumber = (i for i in range(0,2))
print(next(gennumber))
print(next(gennumber))
try:
    print(next(gennumber))
except StopIteration:
    print('last element')
```

　　3.上下文管理器  
　　- with语句实现了上下文管理协议，实现了\__enter__()和\__exit__()方法  
　　- 支持上下文管理协议对象  
```py
file = open('a.txt', encoding='utf8')
try:
    data = file.read()
finally:
    file.close()

with open('a.txt', encoding='utf8') as file:
    data = file.read()
```

##### 5 代码风格
　　1.代码风格  
　　- pylint是Python 代码分析工具，分析代码中错误、代码风格  
　　- pep8与autopep8  
```py
pip install pylint
pip install autopep8
```


###  <font color=indigo>Share: c++ STL库下的迭代器模式</font>
　　迭代器模式（Iterator Design Pattern），也叫作游标模式（Cursor Design Pattern）。  

　　迭代器是用来遍历集合对象。这里说的“集合对象”也可以叫“容器”“聚合对象”，实际上就是包含一组对象的对象，比如数组、链表、树、图、跳表。迭代器模式将集合对象的遍历操作从集合类中拆分出来，放到迭代器类中，让两者的职责更加单一。  

#### **【vector中迭代器的实现和原理】**
　　我们以vector容器为例，来看下迭代器是如何实现的。  

　　来看下vector容器的定义：  
```c++
  /**
   *  @brief A standard container which offers fixed time access to
   *  individual elements in any order.
   *
   *  @ingroup sequences
   *
   *  @tparam _Tp  Type of element.
   *  @tparam _Alloc  Allocator type, defaults to allocator<_Tp>.
   *
   *  Meets the requirements of a <a href="tables.html#65">container</a>, a
   *  <a href="tables.html#66">reversible container</a>, and a
   *  <a href="tables.html#67">sequence</a>, including the
   *  <a href="tables.html#68">optional sequence requirements</a> with the
   *  %exception of @c push_front and @c pop_front.
   *
   *  In some terminology a %vector can be described as a dynamic
   *  C-style array, it offers fast and efficient access to individual
   *  elements in any order and saves the user from worrying about
   *  memory and size allocation.  Subscripting ( @c [] ) access is
   *  also provided as with C-style arrays.
  */
  template<typename _Tp, typename _Alloc = std::allocator<_Tp> >
    class vector : protected _Vector_base<_Tp, _Alloc>
    {
#ifdef _GLIBCXX_CONCEPT_CHECKS
      // Concept requirements.
      typedef typename _Alloc::value_type		_Alloc_value_type;
# if __cplusplus < 201103L
      __glibcxx_class_requires(_Tp, _SGIAssignableConcept)
# endif
      __glibcxx_class_requires2(_Tp, _Alloc_value_type, _SameTypeConcept)
#endif

      typedef _Vector_base<_Tp, _Alloc>			_Base;
      typedef typename _Base::_Tp_alloc_type		_Tp_alloc_type;
      typedef __gnu_cxx::__alloc_traits<_Tp_alloc_type>	_Alloc_traits;

    public:
      typedef _Tp					value_type;
      typedef typename _Base::pointer			pointer;
      typedef typename _Alloc_traits::const_pointer	const_pointer;
      typedef typename _Alloc_traits::reference		reference;
      typedef typename _Alloc_traits::const_reference	const_reference;
      typedef __gnu_cxx::__normal_iterator<pointer, vector> iterator;
      typedef __gnu_cxx::__normal_iterator<const_pointer, vector>
      const_iterator;
      typedef std::reverse_iterator<const_iterator>	const_reverse_iterator;
      typedef std::reverse_iterator<iterator>		reverse_iterator;
      typedef size_t					size_type;
      typedef ptrdiff_t					difference_type;
      typedef _Alloc					allocator_type;

    protected:
      using _Base::_M_allocate;
      using _Base::_M_deallocate;
      using _Base::_M_impl;
      using _Base::_M_get_Tp_allocator;
```

　　从定义中可以看出，vector容器中有四种迭代器iterator，const_iterator，reverse_iterator，const_reverse_iterator，其中带有const的都是只读的。  

　　接下来来看下__normal_iterator的定义和实现代码：  
```c++
template<typename _Iterator, typename _Container>
    class __normal_iterator
    {
    protected:
      _Iterator _M_current;

      typedef iterator_traits<_Iterator>		__traits_type;

    public:
      typedef _Iterator					iterator_type;
      typedef typename __traits_type::iterator_category iterator_category;
      typedef typename __traits_type::value_type  	value_type;
      typedef typename __traits_type::difference_type 	difference_type;
      typedef typename __traits_type::reference 	reference;
      typedef typename __traits_type::pointer   	pointer;

      _GLIBCXX_CONSTEXPR __normal_iterator() _GLIBCXX_NOEXCEPT
      : _M_current(_Iterator()) { }

      explicit
      __normal_iterator(const _Iterator& __i) _GLIBCXX_NOEXCEPT
      : _M_current(__i) { }

...
...

      // Forward iterator requirements
      reference
      operator*() const _GLIBCXX_NOEXCEPT
      { return *_M_current; }

      pointer
      operator->() const _GLIBCXX_NOEXCEPT
      { return _M_current; }

      __normal_iterator&
      operator++() _GLIBCXX_NOEXCEPT
      {
	++_M_current;
	return *this;
      }

      __normal_iterator
      operator++(int) _GLIBCXX_NOEXCEPT
      { return __normal_iterator(_M_current++); }

      // Bidirectional iterator requirements
      __normal_iterator&
      operator--() _GLIBCXX_NOEXCEPT
      {
	--_M_current;
	return *this;
      }

      __normal_iterator
      operator--(int) _GLIBCXX_NOEXCEPT
      { return __normal_iterator(_M_current--); }

      // Random access iterator requirements
      reference
      operator[](difference_type __n) const _GLIBCXX_NOEXCEPT
      { return _M_current[__n]; }

      __normal_iterator&
      operator+=(difference_type __n) _GLIBCXX_NOEXCEPT
      { _M_current += __n; return *this; }

      __normal_iterator
      operator+(difference_type __n) const _GLIBCXX_NOEXCEPT
      { return __normal_iterator(_M_current + __n); }

      __normal_iterator&
      operator-=(difference_type __n) _GLIBCXX_NOEXCEPT
      { _M_current -= __n; return *this; }

      __normal_iterator
      operator-(difference_type __n) const _GLIBCXX_NOEXCEPT
      { return __normal_iterator(_M_current - __n); }

      const _Iterator&
      base() const _GLIBCXX_NOEXCEPT
      { return _M_current; }
    };
```
　　\__normal_iterator的第一个模板参数是_Base::pointer，是个指针，这个指针也就是说__normal_iterator类采用了代理模式，使用了指针的功能。  

　　特别说明的是pointer类型是_Alloc分配器分配的类型的指针。  

　　\__normal_iterator类也重载了很多运算符，包括*、->、++、--、[]。  

　　那么vector是如何使用迭代器呢？  

　　其实vector是通过begin()、end()、rbegin()、rend()来生成相应的迭代器。  
```c++
      // iterators
      /**
       *  Returns a read/write iterator that points to the first
       *  element in the %vector.  Iteration is done in ordinary
       *  element order.
       */
      iterator
      begin() _GLIBCXX_NOEXCEPT
      { return iterator(this->_M_impl._M_start); }

      /**
       *  Returns a read/write iterator that points one past the last
       *  element in the %vector.  Iteration is done in ordinary
       *  element order.
       */
      iterator
      end() _GLIBCXX_NOEXCEPT
      { return iterator(this->_M_impl._M_finish); }

      /**
       *  Returns a read/write reverse iterator that points to the
       *  last element in the %vector.  Iteration is done in reverse
       *  element order.
       */
      reverse_iterator
      rbegin() _GLIBCXX_NOEXCEPT
      { return reverse_iterator(end()); }

      /**
       *  Returns a read/write reverse iterator that points to one
       *  before the first element in the %vector.  Iteration is done
       *  in reverse element order.
       */
      reverse_iterator
      rend() _GLIBCXX_NOEXCEPT
      { return reverse_iterator(begin()); }
```
　　其中this->_M_impl._M_start指向了vector内存池的开头，而this->_M_impl._M_finish则指向了vector中最后一个元素的结尾。  

　　代码我们分析完了，我们来看下迭代器的使用。  

#### **【vector中迭代器的使用】**
　　vector的遍历有三种遍历方式，其中第二种和第三种是一样的，第三种只不过是一种语法糖。  
```c++
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v{1, 2, 3, 4, 5, 6};

    for (auto i = 0; i < v.size(); i++) {
        std::cout << v[i] << std::endl;
    }

    for (auto it = v.begin(); it != v.end(); it++) {
        std::cout << *it << std::endl;
    }

    for (const auto& value : v) {
        std::cout << value << std::endl;
    }
    return 0;
}
```

　　下面重点来了，当我们删除某一个特定元素的时候，改如何处理呢？  

　　常规方法是：  
```c++
int main() {
    std::vector<int> v{1, 2, 3, 3, 4, 5, 6};
    for (auto it = v.begin(); it != v.end(); it++) {
        if (*it == 3) {
            v.erase(it);
        }
    }

    for (const auto& value : v) {
        std::cout << value << std::endl;
    }
    return 0;
}
```

　　结果发现，3并没有全部被删除。  

　　我们来看下源码：  
```c++
      iterator
      erase(const_iterator __position)
      { return _M_erase(begin() + (__position - cbegin())); }
  
  
  
    template<typename _Tp, typename _Alloc>
    typename vector<_Tp, _Alloc>::iterator
    vector<_Tp, _Alloc>::
    _M_erase(iterator __position)
    {
      if (__position + 1 != end())
    _GLIBCXX_MOVE3(__position + 1, end(), __position);
      --this->_M_impl._M_finish;
      _Alloc_traits::destroy(this->_M_impl, this->_M_impl._M_finish);
      return __position;
    }
```

　　在函数erase中，参数是一个const_iterator类型的迭代器，因此程序中it将保持不会；但是删除了一个元素之后，后面的所有元素将会前移，因而it将会指向下一个元素。  

　　设想一下，如果要删除的是最后一个元素，当erase函数返回时，it则等于v.end()；之后it++操作，那么for循环将永远不会退出。何况迭代器还有读操作，会发生未定义行为。  

　　正确的写法应该是：  
```c++
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v{1, 2, 3, 3, 4, 5, 6};
    for (auto it = v.begin(); it != v.end();) {
        if (*it == 3) {
            it = v.erase(it);
        } else {
            it++;
        }
    }

    for (const auto& value : v) {
        std::cout << value << std::endl;
    }
    return 0;
}
```
