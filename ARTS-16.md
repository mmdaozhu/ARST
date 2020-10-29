---
layout: post
title: ARST(十六)
date: 2020-08-03 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=green>Algorithm</font>

#### **【LeetCode:226. Invert Binary Tree】**

##### 　　Description:
　　Invert a binary tree.  

##### 　　Example 1:
```sh
Input:
     4
   /   \
  2     7
 / \   / \
1   3 6   9

Output:

     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

##### 　　Trivia:
　　This problem was inspired by this original tweet by Max Howell:  
　　Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.  

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/226.InvertBinaryTree/InvertBinaryTree.cpp  

###  <font color=green>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
complementary | adj. 补足的
fail-over | n. 故障切换

#### **【Comments and Conclusions】**
　　Active-passive:  
　　With active-passive fail-over, heartbeats are sent between the active and the passive server on standby. If the heartbeat is interrupted, the passive server takes over the active's IP address and resumes service.  
　　Active-passive failover can also be referred to as master-slave failover.  

　　Active-active:  
　　In active-active, both servers are managing traffic, spreading the load between them.  
　　Active-active failover can also be referred to as master-master failover.  

　　Disadvantage(s): failover  
　　Fail-over adds more hardware and additional complexity.  
　　There is a potential for loss of data if the active system fails before any newly written data can be replicated to the passive.  
　
###  <font color=green>tips: 揭秘阿里云创始人王坚的人生经历</font>
https://www.bilibili.com/video/BV1KJ411D7mv  

　　技术人员不仅仅要用技术，更要创造技术。  

　　这个时代是历史上少数的好时代，要把握好时代。  

　　2050大会：让世界各地的年青人因科技而团聚，2050因此成了这群人的公益事业。  

　　城市大脑：一个全新的城市基础设施，不仅仅拥有交通治理，期望解决今天仅靠人脑无法解决的城市发展问题。  

###  <font color=green>Share: boost.Signals2库下的观察者模式</font>
　　观察者模式：在对象之间定义一个一对多的依赖，当一个对象状态改变的时候，所有依赖的对象都会自动收到通知。  
　　一般情况下，被依赖的对象叫作被观察者（Observable），依赖的对象叫作观察者（Observer）。  

#### **【Signals2库简介】**
　　Signals2实现了线程安全的观察者模式，被称为信号/插槽，它是一种函数回调机制，一个信号关联了多个插槽，当信号发出时，所有关联它的插槽都会被调用。  

#### **【signal类摘要】**
```c++
template<typename Signature, 
         typename Combiner = boost::signals2::optional_last_value<R>, 
         typename Group = int, typename GroupCompare = std::less<Group>, 
         typename SlotFunction = boost::function<Signature>, 
         typename ExtendedSlotFunction = boost::function<R (const connection &, T1, T2, ..., TN)>, 
         typename Mutex = boost::signals2::mutex> 
class signal : public boost::signals2::signal_base {
public:
  // types
  typedef Signature                                                                          signature_type;             
  typedef typename Combiner::result_type                                                     result_type;                
  typedef Combiner                                                                           combiner_type;              
  typedef Group                                                                              group_type;                 
  typedef GroupCompare                                                                       group_compare_type;         
  typedef SlotFunction                                                                       slot_function_type;         
  typedef typename signals2::slot<Signature, SlotFunction>                                   slot_type;                  
  typedef ExtendedSlotFunction                                                               extended_slot_function_type;
  typedef typename signals2::slot<R (const connection &, T1, ..., TN), ExtendedSlotFunction> extended_slot_type;         
  typedef typename SlotFunction::result_type                                                 slot_result_type;           
  typedef unspecified                                                                        slot_call_iterator;         
  typedef T1                                                                                 argument_type;                // Exists iff arity == 1
  typedef T1                                                                                 first_argument_type;          // Exists iff arity == 2
  typedef T2                                                                                 second_argument_type;         // Exists iff arity == 2

  // static constants
  static const int arity = N;  // The number of arguments taken by the signal.

  // member classes/structs/unions
  template<unsigned n> 
  class arg {
  public:
    // types
    typedef Tn type;  // The type of the signal's (n+1)th argument
  };

  // construct/copy/destruct
  signal(const combiner_type& = combiner_type(), 
         const group_compare_type& = group_compare_type());
  signal(signal &&);
  signal& operator=(signal &&);

  // connection management
  connection connect(const slot_type&, connect_position = at_back);
  connection connect(const group_type&, const slot_type&, 
                     connect_position = at_back);
  connection connect_extended(const extended_slot_type&, 
                              connect_position = at_back);
  connection connect_extended(const group_type&, const extended_slot_type&, 
                              connect_position = at_back);
  void disconnect(const group_type&);
  template<typename S> void disconnect(const S&);
  void disconnect_all_slots();
  bool empty() const;
  std::size_t num_slots() const;

  // invocation
  result_type operator()(arg<0>::type, arg<1>::type, ..., arg<N-1>::type);
  result_type operator()(arg<0>::type, arg<1>::type, ..., arg<N-1>::type) const;

  // combiner access
  combiner_type combiner() const;
  void set_combiner(const combiner_type&);

  // modifiers
  void swap(signal&);
};
```
　　第一个模板参数Signature的含义和function的一模一样，也是一个函数类型签名。例如：  
```c++
signal<void(int,double)>
```
　　第二个模板参数Combiner是一个函数对象，它被称为“合并器”，用来组合所有插槽的调用结果。默认是boost::signals2::optional_last_value<R>，返回最后一个被调用的插槽返回值。  
　　第三个模板参数Group是插槽编组的类型，缺省使用int来标记组号。  

#### **【signal类的操作函数】**
　　signal最重要的操作函数是插槽管理connect()函数，它把插槽连接到信号上，相当于为信号增加一个处理的handler。  
　　插槽可以是任何可调物，signal内部使用function作为容器保存这些可调物对象。连接时可以指定组号，也可以不指定组号，当信号发生时将依据组号的排序准则依次调用插槽函数。  

#### **【插槽的连接与调用】**
　　示例代码如下：
```c++
#include <boost/signals2.hpp>
#include <iostream>

struct Hello {
    void operator()() const { std::cout << "Hello" << std::endl; }
};

struct World {
    void operator()() const { std::cout << "World" << std::endl; }
};

int main() {
    boost::signals2::signal<void()> sig;
    sig.connect(Hello());
    sig.connect(World());

    sig();
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// Hello
// World
```
　　signal就像一个增强的function对象，它可以容纳（使用connect()连接）多个符合模板参数中函数签名类型的函数（插槽），形成一个插槽链表，然后在信号发生时一起调用。  

#### **【使用组号】**
　　如果在连接的时候指定组号，那么每个编组的插槽将是又一个插槽链表，形成一个复杂的二维链表，它们的顺序规则如下：  
　　1.各编组的调用顺序由组号从小到大决定。  
　　2.每个编组的插槽内部链表插入顺序用at_back和at_front指定。  
　　3.未被编组的插槽如果位置标志是at_front，将在所有的编组之前调用。  
　　4.未被编组的插槽如果位置标志是at_back，将在所有的编组之后调用。  
　　示例代码如下：  
```c++
#include <boost/signals2.hpp>
#include <iostream>

template <int N>
struct slots {
    void operator()() { std::cout << "slot" << N << std::endl; }
};

int main() {
    boost::signals2::signal<void()> sig;
    sig.connect(slots<1>(), boost::signals2::at_back);
    sig.connect(slots<100>(), boost::signals2::at_front);
    sig.connect(1, slots<11>(), boost::signals2::at_back);
    sig.connect(1, slots<12>(), boost::signals2::at_front);
    sig.connect(2, slots<21>(), boost::signals2::at_back);
    sig.connect(2, slots<22>(), boost::signals2::at_front);
    sig();
    return 0;
}

// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// slot100
// slot12
// slot11
// slot22
// slot21
// slot1
```

#### **【信号的参数和返回值】**
　　signal如function一样，不仅可以把输入参数转发给所有插槽，也可以传回插槽的返回值。默认情况下signal使用合并器optional_last_value<R>,它将使用optional对象返回最后被调用的插槽的返回值。  
　　示例代码如下：
```c++
#include <boost/signals2.hpp>
#include <iostream>

template <int N>
struct slots {
    int operator()(int x) {
        std::cout << "slot" << N << std::endl;
        return x * N;
    }
};

int main() {
    boost::signals2::signal<int(int)> sig;
    sig.connect(slots<10>());
    sig.connect(slots<20>());
    sig.connect(slots<30>());
    int last = *sig(10);
    std::cout << "last value: " << last << std::endl;
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// slot10
// slot20
// slot30
// last value: 300
```

#### **【合并器】**
　　有时候我们比较关心每一个插槽的返回值，这个时候就需要自定义合并器来处理返回值。  
　　合并器其实就是个函数对象。形式如下：  
```c++
template <typename T>
struct Combiner {
    typedef T result_type;
    template <typename InputIterator>
    T operator()(InputIterator first, InputIterator last) const ;
};
```
　　接下来我们来自定义一个合并器，来计算各个插槽返回值之和，代码如下：  
```c++
#include <boost/signals2.hpp>
#include <iostream>

template <int N>
struct slots {
    int operator()(int x) {
        std::cout << "slot" << N << std::endl;
        return x * N;
    }
};

template <typename T>
struct Sum {
    typedef T result_type;
    template <typename InputIterator>
    T operator()(InputIterator first, InputIterator last) const {
        if (first == last) {
            return T();
        }
        T sum = *first++;
        while (first != last) {
            sum += *first;
            first++;
        }
        return sum;
    }
};

int main() {
    boost::signals2::signal<int(int), Sum<int>> sig;
    sig.connect(slots<10>());
    sig.connect(slots<20>());
    sig.connect(slots<30>());
    auto s = sig(2);
    std::cout << s << std::endl;
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// slot10
// slot20
// slot30
// 60
```

#### **【管理信号的连接】**
　　signal使用connect()连接插槽时，它就会返回一个connection对象，可以管理连接。  
　　connection类的定义如下：  
```c++
class connection
    {
    public:
      friend class shared_connection_block;

      connection() {}
      connection(const connection &other): _weak_connection_body(other._weak_connection_body)
      {}
      connection(const boost::weak_ptr<detail::connection_body_base> &connectionBody):
        _weak_connection_body(connectionBody)
      {}
      
      // move support
#if !defined(BOOST_NO_CXX11_RVALUE_REFERENCES)
      connection(connection && other): _weak_connection_body(std::move(other._weak_connection_body))
      {
        // make sure other is reset, in case it is a scoped_connection (so it
        // won't disconnect on destruction after being moved away from).
        other._weak_connection_body.reset();
      }
      connection & operator=(connection && other)
      {
        if(&other == this) return *this;
        _weak_connection_body = std::move(other._weak_connection_body);
        // make sure other is reset, in case it is a scoped_connection (so it
        // won't disconnect on destruction after being moved away from).
        other._weak_connection_body.reset();
        return *this;
      }
#endif // !defined(BOOST_NO_CXX11_RVALUE_REFERENCES)
      connection & operator=(const connection & other)
      {
        if(&other == this) return *this;
        _weak_connection_body = other._weak_connection_body;
        return *this;
      }

      ~connection() {}
      void disconnect() const
      {
        boost::shared_ptr<detail::connection_body_base> connectionBody(_weak_connection_body.lock());
        if(connectionBody == 0) return;
        connectionBody->disconnect();
      }
      bool connected() const
      {
        boost::shared_ptr<detail::connection_body_base> connectionBody(_weak_connection_body.lock());
        if(connectionBody == 0) return false;
        return connectionBody->connected();
      }
      bool blocked() const
      {
        boost::shared_ptr<detail::connection_body_base> connectionBody(_weak_connection_body.lock());
        if(connectionBody == 0) return true;
        return connectionBody->blocked();
      }
      bool operator==(const connection& other) const
      {
        boost::shared_ptr<detail::connection_body_base> connectionBody(_weak_connection_body.lock());
        boost::shared_ptr<detail::connection_body_base> otherConnectionBody(other._weak_connection_body.lock());
        return connectionBody == otherConnectionBody;
      }
      bool operator!=(const connection& other) const
      {
        return !(*this == other);
      }
      bool operator<(const connection& other) const
      {
        boost::shared_ptr<detail::connection_body_base> connectionBody(_weak_connection_body.lock());
        boost::shared_ptr<detail::connection_body_base> otherConnectionBody(other._weak_connection_body.lock());
        return connectionBody < otherConnectionBody;
      }
      void swap(connection &other)
      {
        using std::swap;
        swap(_weak_connection_body, other._weak_connection_body);
      }
    protected:

      boost::weak_ptr<detail::connection_body_base> _weak_connection_body;
    };
```
　　connection类的成员函数disconnect()和connected()分别用来信号断开连接和检测连接状态。  
```c++
#include <boost/signals2.hpp>
#include <iostream>

template <int N>
struct slots {
    int operator()(int x) {
        std::cout << "slot" << N << std::endl;
        return x * N;
    }
};

int main() {
    boost::signals2::signal<int(int)> sig;
    boost::signals2::connection c1 = sig.connect(slots<10>());
    boost::signals2::connection c2 = sig.connect(slots<20>());
    boost::signals2::connection c3 = sig.connect(slots<30>());

    c1.disconnect();
    std::cout << sig.num_slots() << std::endl;
    std::cout << c1.connected() << std::endl;
    std::cout << c2.connected() << std::endl;
    sig(2);
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 2
// 0
// 1
// slot20
// slot30
```

#### **【自动连接管理】**
　　如果插槽在与信号建立连接后被意外地销毁了，那么信号调用将发生未定义行为。  
　　因此signals2库使用slot类提供了自动连接管理地功能，能够自动跟踪插槽地生命周期，当插槽失效时会自动断开连接。  
```c++
#include <boost/make_shared.hpp>
#include <boost/ref.hpp>
#include <boost/signals2.hpp>
#include <iostream>

template <int N>
struct slots {
    int operator()(int x) {
        std::cout << "slot" << N << std::endl;
        return x * N;
    }
};

int main() {
    typedef boost::signals2::signal<int(int)> signal_t;
    signal_t sig;
    auto p = boost::make_shared<slots<10> >();
    sig.connect(signal_t::slot_type(boost::ref(*p)).track(p));
    p.reset();
    sig(2);
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
```

#### **【观察者模式的应用】**
　　示例代码如下：
```c++
#include <boost/bind/bind.hpp>
#include <boost/make_shared.hpp>
#include <boost/ref.hpp>
#include <boost/signals2.hpp>
#include <iostream>

class ring {
public:
    using signal_t = boost::signals2::signal<void()>;
    using slot_t = signal_t::slot_type;
    using connection = boost::signals2::connection;
    connection connect(const slot_t& s) { return alarm.connect(s); }

    void press() {
        std::cout << "ring alarm..." << std::endl;
        alarm();
    }

private:
    signal_t alarm;
};

extern const char nurse1[] = "Mary";
extern const char nurse2[] = "Kate";
template <const char* name>
class nurse {
public:
    void action() {
        std::cout << name << std::endl;
        std::cout << "wakeup and open door" << std::endl;
    }
};

class guest {
public:
    void press(ring& r) {
        std::cout << "A guest press the ring." << std::endl;
        r.press();
    }
};

int main() {
    ring r;
    nurse<nurse1> n1;
    nurse<nurse2> n2;
    guest g;
    r.connect(boost::bind(&nurse<nurse1>::action, n1));
    r.connect(boost::bind(&nurse<nurse2>::action, n2));

    g.press(r);
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
A guest press the ring.
ring alarm...
Mary
wakeup and open door
Kate
wakeup and open door
```