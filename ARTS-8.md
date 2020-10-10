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
https://github.com/mmdaozhu/leetcode/blob/master/cpp/641.DesignCircularDeque/DesignCircularDeque.cpp

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
　
###  <font color=orange>tips：Java 注解简介</font>
　　初学Spring Boot时，发现有好多注解，看的眼花缭乱的，所以想把这块学一学，了解一下其机制。  
　　学习链接https://www.runoob.com/w3cnote/java-annotation.html  

#### **【Java 注解定义】**
　　Java 注解（Annotation）又称 Java 标注，是 JDK5.0 引入的一种注释机制。  
　　Java语言中的类、方法、变量、参数和包等都可以被标注。和Javadoc不同，Java标注可以通过反射获取标注内容。在编译器生成类文件时，标注可以被嵌入到字节码中。Java虚拟机可以保留标注内容，在运行时可以获取到标注内容。当然它也支持自定义 Java 标注。  

#### **【内置的注解】**
　　- @Override - 检查该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。  
　　- @Deprecated - 标记过时方法。如果使用该方法，会报编译警告。  
　　- @SuppressWarnings - 指示编译器去忽略注解中声明的警告。  

　　作用在其他注解的注解(或者说 元注解)是:  
　　- @Retention - 标识这个注解怎么保存，是只在代码中，还是编入class文件中，或者是在运行时可以通过反射访问。  
　　- @Documented - 标记这些注解是否包含在用户文档中。  
　　- @Target - 标记这个注解应该是哪种 Java 成员。  
　　- @Inherited - 标记这个注解是继承于哪个注解类(默认 注解并没有继承于任何子类)  
　　- @SafeVarargs - Java 7 开始支持，忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。  
　　- @FunctionalInterface - Java 8 开始支持，标识一个匿名函数或函数式接口。  
　　- @Repeatable - Java 8 开始支持，标识某注解可以在同一个声明上使用多次。  

#### **【Annotation 架构】**
![hello](/ARTS-8/1.jpg)  
　　1. Annotation 就是个接口。  
　　2. ElementType 是 Enum 枚举类型，它用来指定 Annotation 的类型。例如Java语言中的类、方法、变量、参数和包等被标注。  
```java
package java.lang.annotation;
public enum ElementType {
    TYPE,               /* 类、接口（包括注释类型）或枚举声明  */
    FIELD,              /* 字段声明（包括枚举常量）  */
    METHOD,             /* 方法声明  */
    PARAMETER,          /* 参数声明  */
    CONSTRUCTOR,        /* 构造方法声明  */
    LOCAL_VARIABLE,     /* 局部变量声明  */
    ANNOTATION_TYPE,    /* 注释类型声明  */
    PACKAGE             /* 包声明  */
}
```
　　3. RetentionPolicy 是 Enum 枚举类型，它用来指定 Annotation 的策略。通俗点说，就是不同 RetentionPolicy 类型的 Annotation 的作用域不同。  
```java
package java.lang.annotation;
public enum RetentionPolicy {
    SOURCE,            /* Annotation信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了  */
    CLASS,             /* 编译器将Annotation存储于类对应的.class文件中。默认行为  */
    RUNTIME            /* 编译器将Annotation存储于class文件中，并且可由JVM读入 */
}
```

#### **【Annotation 的作用】**

##### 　　1.编译检查
　　Annotation 具有"让编译器进行编译检查的作用"。  
　　例如，@SuppressWarnings, @Deprecated 和 @Override 都具有编译检查作用。  

##### 　　2.在反射中使用 Annotation
　　在反射的 Class, Method, Field 等函数中，有许多于 Annotation 相关的接口。  
　　这也意味着，我们可以在反射中解析并使用 Annotation。  
```java
import java.lang.annotation.Annotation;
import java.lang.annotation.Target;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Inherited;
import java.lang.reflect.Method;

/**
 * Annotation在反射函数中的使用示例
 */
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation {
    String[] value() default "unknown";
}

/**
 * Person类。它会使用MyAnnotation注解。
 */
class Person {
   
    /**
     * empty()方法同时被 "@Deprecated" 和 "@MyAnnotation(value={"a","b"})"所标注
     * (01) @Deprecated，意味着empty()方法，不再被建议使用
     * (02) @MyAnnotation, 意味着empty() 方法对应的MyAnnotation的value值是默认值"unknown"
     */
    @MyAnnotation
    @Deprecated
    public void empty(){
        System.out.println("\nempty");
    }
   
    /**
     * sombody() 被 @MyAnnotation(value={"girl","boy"}) 所标注，
     * @MyAnnotation(value={"girl","boy"}), 意味着MyAnnotation的value值是{"girl","boy"}
     */
    @MyAnnotation(value={"girl","boy"})
    public void somebody(String name, int age){
        System.out.println("\nsomebody: "+name+", "+age);
    }
}

public class AnnotationTest {

    public static void main(String[] args) throws Exception {
        // 新建Person
        Person person = new Person();
        // 获取Person的Class实例
        Class<Person> c = Person.class;
        // 获取 somebody() 方法的Method实例
        Method mSomebody = c.getMethod("somebody", new Class[]{String.class, int.class});
        // 执行该方法
        mSomebody.invoke(person, new Object[]{"lily", 18});
        iteratorAnnotations(mSomebody);

        // 获取 somebody() 方法的Method实例
        Method mEmpty = c.getMethod("empty", new Class[]{});
        // 执行该方法
        mEmpty.invoke(person, new Object[]{});        
        iteratorAnnotations(mEmpty);
    }
   
    public static void iteratorAnnotations(Method method) {
        // 判断 somebody() 方法是否包含MyAnnotation注解
        if(method.isAnnotationPresent(MyAnnotation.class)){
            // 获取该方法的MyAnnotation注解实例
            MyAnnotation myAnnotation = method.getAnnotation(MyAnnotation.class);
            // 获取 myAnnotation的值，并打印出来
            String[] values = myAnnotation.value();
            for (String str:values)
                System.out.printf(str+", ");
            System.out.println();
        }
       
        // 获取方法上的所有注解，并打印出来
        Annotation[] annotations = method.getAnnotations();
        for(Annotation annotation : annotations){
            System.out.println(annotation);
        }
    }
}
// result:
// somebody: lily, 18
// girl, boy, 
// @com.skywang.annotation.MyAnnotation(value=[girl, boy])

// empty
// unknown, 
// @com.skywang.annotation.MyAnnotation(value=[unknown])
// @java.lang.Deprecated()
```

##### 　　3.根据 Annotation 生成帮助文档
　　通过给 Annotation 注解加上 @Documented 标签，能使该 Annotation 标签出现在 javadoc 中。  

##### 　　4.能够帮忙查看查看代码
　　通过 @Override, @Deprecated 等，我们能很方便的了解程序的大致结构。  

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