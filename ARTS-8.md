---
layout: post
title: ARST(八)
date: 2020-06-08 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=orange>Algorithm</font>

#### **【LeetCode:641. Design Circular Deque】**

##### 　　Description:
　　Design your implementation of the circular double-ended queue (deque).  
　　Your implementation should support following operations:  
　　- MyCircularDeque(k): Constructor, set the size of the deque to be k.  
　　- insertFront(): Adds an item at the front of Deque. Return true if the operation is successful.  
　　- insertLast(): Adds an item at the rear of Deque. Return true if the operation is successful.  
　　- deleteFront(): Deletes an item from the front of Deque. Return true if the operation is successful.  
　　- deleteLast(): Deletes an item from the rear of Deque. Return true if the operation is successful.  
　　- getFront(): Gets the front item from the Deque. If the deque is empty, return -1.  
　　- getRear(): Gets the last item from Deque. If the deque is empty, return -1.  
　　- isEmpty(): Checks whether Deque is empty or not.   
　　- isFull(): Checks whether Deque is full or not.  

##### 　　Note:
　　- All values will be in the range of [0, 1000].  
　　- The number of operations will be in the range of [1, 1000].  
　　- Please do not use the built-in Deque library.  

##### 　　Example:
```sh
MyCircularDeque circularDeque = new MycircularDeque(3); // set the size to be 3
circularDeque.insertLast(1);			// return true
circularDeque.insertLast(2);			// return true
circularDeque.insertFront(3);			// return true
circularDeque.insertFront(4);			// return false, the queue is full
circularDeque.getRear();  			// return 2
circularDeque.isFull();				// return true
circularDeque.deleteLast();			// return true
circularDeque.insertFront(4);			// return true
circularDeque.getFront();			// return 4
```

##### 　　C++ Solution:
```cpp
#include <assert.h>

#include <iostream>

/*
解体思路：
    设计循环双端队列最关键的是判空和判满的条件。
    判空：front == rear;
    判满：(rear + 1) % size == front;
*/

class MyCircularDeque {
public:
    /** Initialize your data structure here. Set the size of the deque to be k. */
    MyCircularDeque(int k) {
        deque = new int[k + 1];
        size = k + 1;
        rear = front = 0;
    }

    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    bool insertFront(int value) {
        if (isFull()) {
            return false;
        }
        front = (front - 1 + size) % size;
        deque[front] = value;
        return true;
    }

    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    bool insertLast(int value) {
        if (isFull()) {
            return false;
        }
        deque[rear] = value;
        rear = (rear + 1) % size;
        return true;
    }

    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    bool deleteFront() {
        if (isEmpty()) {
            return false;
        }
        front = (front + 1) % size;
        return true;
    }

    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    bool deleteLast() {
        if (isEmpty()) {
            return false;
        }
        rear = (rear - 1 + size) % size;
        return true;
    }

    /** Get the front item from the deque. */
    int getFront() {
        if (isEmpty()) {
            return -1;
        }
        return deque[front];
    }

    /** Get the last item from the deque. */
    int getRear() {
        if (isEmpty()) {
            return -1;
        }
        return deque[(rear - 1 + size) % size];
    }

    /** Checks whether the circular deque is empty or not. */
    bool isEmpty() { return front == rear; }

    /** Checks whether the circular deque is full or not. */
    bool isFull() { return ((rear + 1) % size) == front; }

private:
    int* deque;
    int size;
    int front;
    int rear;
};
```

###  <font color=orange>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer (https://www.youtube.com/watch?v=-W9F__D3oY4)

#### **【Comments and Conclusions】**

##### 1. Load Balancers
　　Software：  
　　- ELB(Elastic Load Balancing - Amazon Web Services)  
　　- HAProxy  
　　- LVS(Linux Virtual Server,)  
　　- ...  

　　Hardware：  
　　- Barracuda  
　　- Cisco  
　　- Citrix  
　　- F5  
　　- ...  

##### 2. Sticky Sessions

##### 3. PHP Acceleration
　　- Code Optimization  
　　- Opcode Caching  
　　- ...  

##### 4. Caching
　　- .html  
　　- MySQL Query Cache  
　　- memcached  
　　- ...  
　
###  <font color=orange>tips：</font>

###  <font color=orange>Share：简析Boost库中的单例模式</font>
　　首先非常遗憾的是Boost库中的没有专门的单例库，但是在其他库中有并不十分完善的实现。  

#### **【boost.container中单例实现】**
　　singleton_default的定义如下：  
```c++
namespace boost {
namespace container {
namespace dtl {

// T must be: no-throw default constructible and no-throw destructible
template <typename T>
struct singleton_default
{
  private:
    struct object_creator
    {
      // This constructor does nothing more than ensure that instance()
      //  is called before main() begins, thus creating the static
      //  T object before multithreading race issues can come up.
      object_creator() { singleton_default<T>::instance(); }
      inline void do_nothing() const { }
    };
    static object_creator create_object;

    singleton_default();

  public:
    typedef T object_type;

    // If, at any point (in user code), singleton_default<T>::instance()
    //  is called, then the following function is instantiated.
    static object_type & instance()
    {
      // This is the object that we return a reference to.
      // It is guaranteed to be created before main() begins because of
      //  the next line.
      static object_type obj;

      // The following line does nothing else than force the instantiation
      //  of singleton_default<T>::create_object, whose constructor is
      //  called before main() begins.
      create_object.do_nothing();

      return obj;
    }
};
template <typename T>
typename singleton_default<T>::object_creator
singleton_default<T>::create_object;

} // namespace dtl
} // namespace container
} // namespace boost
```
　　singleton_default把模板参数T实现为一个单例类，唯一实例只能通过静态成员函数instance()访问，对类型T的要求是有缺省构造函数，并且在构造和解析时不能抛出异常。  
　　singleton_default使用一个object_creator类型的static成员变量，在main函数之前执行object_creator构造函数来保证单例在main之前被构造好。  
　　单例在main函数之前创建的，因此在多线程环境下是唯一的。  
　　这里面最迷惑的地方是create_object.do_nothing()，这么用主要是强制singleton_default<T>::create_object执行初始化操作，因为非局部变量的初始化是否发生在main函数之前完全依赖于编译器的实现。  
　　这是饿汉式的单例模式，也是最常用的单例模式。  
　　用法：
```c++
#include <boost/container/detail/singleton.hpp>
#include <iostream>

class Point {
public:
    Point(int x = 0, int y = 0) : x(x), y(x) { std::cout << "point constructor" << std::endl; }
    ~Point() { std::cout << "point destructor" << std::endl; }
    void print() {
        std::cout << "x: " << x << ", "
                  << "y: " << y << std::endl;
    }

private:
    int x;
    int y;
};

int main() {
    std::cout << "main() start" << std::endl;
    boost::container::dtl::singleton_default<Point>::instance().print();
    std::cout << "main() end" << std::endl;
    return 0;
}

// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// point constructor
// main() start
// x: 0, y: 0
// main() end
// point destructor
```

#### **【boost.serialization中单例实现】**
　　singleton的定义如下：  
```c++
namespace boost {
namespace serialization {

//////////////////////////////////////////////////////////////////////
// Provides a dynamically-initialized (singleton) instance of T in a
// way that avoids LNK1179 on vc6.  See http://tinyurl.com/ljdp8 or
// http://lists.boost.org/Archives/boost/2006/05/105286.php for
// details.
//

// Singletons created by this code are guaranteed to be unique
// within the executable or shared library which creates them.
// This is sufficient and in fact ideal for the serialization library.
// The singleton is created when the module is loaded and destroyed
// when the module is unloaded.

// This base class has two functions.

// First it provides a module handle for each singleton indicating
// the executable or shared library in which it was created. This
// turns out to be necessary and sufficient to implement the tables
// used by serialization library.

// Second, it provides a mechanism to detect when a non-const function
// is called after initialization.

// Make a singleton to lock/unlock all singletons for alteration.
// The intent is that all singletons created/used by this code
// are to be initialized before main is called. A test program
// can lock all the singletons when main is entered.  Thus any
// attempt to retrieve a mutable instance while locked will
// generate an assertion if compiled for debug.

// The singleton template can be used in 2 ways:
// 1 (Recommended): Publicly inherit your type T from singleton<T>,
// make its ctor protected and access it via T::get_const_instance()
// 2: Simply access singleton<T> without changing T. Note that this only
// provides a global instance accesible by singleton<T>::get_const_instance()
// or singleton<T>::get_mutable_instance() to prevent using multiple instances
// of T make its ctor protected

// Note on usage of BOOST_DLLEXPORT: These functions are in danger of
// being eliminated by the optimizer when building an application in
// release mode. Usage of the macro is meant to signal the compiler/linker
// to avoid dropping these functions which seem to be unreferenced.
// This usage is not related to autolinking.

class BOOST_SYMBOL_VISIBLE singleton_module :
    public boost::noncopyable
{
private:
    BOOST_DLLEXPORT bool & get_lock() BOOST_USED {
        static bool lock = false;
        return lock;
    }

public:
    BOOST_DLLEXPORT void lock(){
        get_lock() = true;
    }
    BOOST_DLLEXPORT void unlock(){
        get_lock() = false;
    }
    BOOST_DLLEXPORT bool is_locked(){
        return get_lock();
    }
};

static inline singleton_module & get_singleton_module(){
    static singleton_module m;
    return m;
}

namespace detail {

// This is the class actually instantiated and hence the real singleton.
// So there will only be one instance of this class. This does not hold
// for singleton<T> as a class derived from singleton<T> could be
// instantiated multiple times.
// It also provides a flag `is_destroyed` which returns true, when the
// class was destructed. It is static and hence accesible even after
// destruction. This can be used to check, if the singleton is still
// accesible e.g. in destructors of other singletons.
template<class T>
class singleton_wrapper : public T
{
    static bool & get_is_destroyed(){
        // Prefer a static function member to avoid LNK1179.
        // Note: As this is for a singleton (1 instance only) it must be set
        // never be reset (to false)!
        static bool is_destroyed_flag = false;
        return is_destroyed_flag;
    }
public:
    singleton_wrapper(){
        BOOST_ASSERT(! is_destroyed());
    }
    ~singleton_wrapper(){
        get_is_destroyed() = true;
    }
    static bool is_destroyed(){
        return get_is_destroyed();
    }
};

} // detail

template <class T>
class singleton {
private:
    static T * m_instance;
    // include this to provoke instantiation at pre-execution time
    static void use(T const &) {}
    static T & get_instance() {
        BOOST_ASSERT(! is_destroyed());

        // use a wrapper so that types T with protected constructors can be used
        // Using a static function member avoids LNK1179
        static detail::singleton_wrapper< T > t;

        // note that the following is absolutely essential.
        // commenting out this statement will cause compilers to fail to
        // construct the instance at pre-execution time.  This would prevent
        // our usage/implementation of "locking" and introduce uncertainty into
        // the sequence of object initialization.
        // Unfortunately, this triggers detectors of undefine behavior
        // and reports an error.  But I've been unable to find a different
        // of guarenteeing that the the singleton is created at pre-main time.
        if (m_instance) use(* m_instance);

        return static_cast<T &>(t);
    }
protected:
    // Do not allow instantiation of a singleton<T>. But we want to allow
    // `class T: public singleton<T>` so we can't delete this ctor
    BOOST_DLLEXPORT singleton(){}

public:
    BOOST_DLLEXPORT static T & get_mutable_instance(){
        BOOST_ASSERT(! get_singleton_module().is_locked());
        return get_instance();
    }
    BOOST_DLLEXPORT static const T & get_const_instance(){
        return get_instance();
    }
    BOOST_DLLEXPORT static bool is_destroyed(){
        return detail::singleton_wrapper< T >::is_destroyed();
    }
};

// Assigning the instance reference to a static member forces initialization
// at startup time as described in
// https://groups.google.com/forum/#!topic/microsoft.public.vc.language/kDVNLnIsfZk
template<class T>
T * singleton< T >::m_instance = & singleton< T >::get_instance();

} // namespace serialization
} // namespace boost
```
　　singleton和container.singleton_default基本相同，对类型T的要求也是要有缺省构造函数，并且在构造和解析时不能抛出异常。  
　　singleton的访问单例实例的成员函数有点不同，分别有get_const_instance()，get_mutable_instance()，获取可变对象单例不是线程安全的，可能会发生线程竞争问题。  
　　BOOST_ASSERT仅在debug模式下才会生效，主要是测试修改单例地方。  
　　这也是饿汉式的单例模式。  
　　用法：  
```c++
#include <boost/serialization/singleton.hpp>
#include <iostream>

class Point : public boost::serialization::singleton<Point> {
protected:
    Point(int x = 0, int y = 0) : x(x), y(x) { std::cout << "point constructor" << std::endl; }
    ~Point() { std::cout << "point destructor" << std::endl; }

public:
    void print() const {
        std::cout << "x: " << x << ", "
                  << "y: " << y << std::endl;
    }

private:
    int x;
    int y;
};

int main() {
    std::cout << "main() start" << std::endl;
    Point::get_const_instance().print();
    std::cout << "main() end" << std::endl;
    return 0;
}

// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// point constructor
// main() start
// x: 0, y: 0
// main() end
// point destructor
```

#### **【boost.thread中单例实现】**
　　singleton的定义如下：  
```c++
namespace boost {
namespace detail {
namespace thread {

// class singleton has the same goal as all singletons: create one instance of
// a class on demand, then dish it out as requested.

template <class T>
class singleton : private T
{
private:
    singleton();
    ~singleton();

public:
    static T &instance();
};


template <class T>
inline singleton<T>::singleton()
{
    /* no-op */
}

template <class T>
inline singleton<T>::~singleton()
{
    /* no-op */
}

template <class T>
/*static*/ T &singleton<T>::instance()
{
    // function-local static to force this to work correctly at static
    // initialization time.
    static singleton<T> s_oT;
    return(s_oT);
}

} // namespace thread
} // namespace detail
} // namespace boost
```
　　boost.thread中单例实现非常简单，只不过是单例在第一次访问时才构造，因此在多线程环境中要格外注意。  
　　这是懒汉式的单例模式，不过不是线程安全的。  
　　用法：  
```c++
#include <boost/thread/detail/singleton.hpp>
#include <iostream>

class Point {
protected:
    Point(int x = 0, int y = 0) : x(x), y(x) { std::cout << "point constructor" << std::endl; }
    ~Point() { std::cout << "point destructor" << std::endl; }

public:
    void print() const {
        std::cout << "x: " << x << ", "
                  << "y: " << y << std::endl;
    }

private:
    int x;
    int y;
};

int main() {
    std::cout << "main() start" << std::endl;
    boost::detail::thread::singleton<Point>::instance().print();
    std::cout << "main() end" << std::endl;
    return 0;
}

// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// main() start
// point constructor
// x: 0, y: 0
// main() end
// point destructor
```