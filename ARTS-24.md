---
layout: post
title: ARST(二十四)
date: 2020-09-28 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=indigo>Algorithm</font>

#### **【LeetCode:127. Word Ladder】**
　　Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:  

　　1. Only one letter can be changed at a time.  
　　2. Each transformed word must exist in the word list.  

　　Note:  
　　- Return 0 if there is no such transformation sequence.  
　　- All words have the same length.  
　　- All words contain only lowercase alphabetic characters.  
　　- You may assume no duplicates in the word list.  
　　- You may assume beginWord and endWord are non-empty and are not the same.  

##### 　　Example 1:
```sh
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

##### 　　Example 2:
```sh
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/127.WordLadder/WordLadder.cpp  

###  <font color=indigo>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
Sharding | n. 分片；分区
lopsided | adj. 不平衡的，倾向一方的
Denormalization | n. 反规范化；[经] 反向规格化
circumvent | v. 包围；智取；绕行，规避
outnumber | vt. 数目超过；比…多
tuning | n. 调音；起音，定音；音调


#### **【Comments and Conclusions】**

##### Relational database management system (RDBMS)

##### Sharding
　　Sharding distributes data across different databases such that each database can only manage a subset of the data. Taking a users database as an example, as the number of users increases, more shards are added to the cluster.  

　　sharding results in less read and write traffic, less replication, and more cache hits. Index size is also reduced, which generally improves performance with faster queries. If one shard goes down, the other shards are still operational, although you'll want to add some form of replication to avoid data loss.  

　　Disadvantage(s):   

　　- You'll need to update your application logic to work with shards, which could result in complex SQL queries.  
　　- Data distribution can become lopsided in a shard.  
　　- Joining data from multiple shards is more complex.  
　　- Sharding adds more hardware and additional complexity.  

##### Denormalization
　　Denormalization attempts to improve read performance at the expense of some write performance. Redundant copies of the data are written in multiple tables to avoid expensive joins.   
　　Once data becomes distributed with techniques such as federation and sharding, managing joins across data centers further increases complexity. Denormalization might circumvent the need for such complex joins.  
　　In most systems, reads can heavily outnumber writes 100:1 or even 1000:1. A read resulting in a complex database join can be very expensive, spending a significant amount of time on disk operations.  

　　Disadvantage(s):   
　　- Data is duplicated.  
　　- Constraints can help redundant copies of information stay in sync, which increases complexity of the database design.  
　　- A denormalized database under heavy write load might perform worse than its normalized counterpart.  

##### SQL tuning
　　It's important to benchmark and profile to simulate and uncover bottlenecks.  

　　- Benchmark - Simulate high-load situations with tools such as ab.  
　　- Profile - Enable tools such as the slow query log to help track performance issues.  

　　Benchmarking and profiling might point you to the following optimizations.  

　　Tighten up the schema:  
　　- MySQL dumps to disk in contiguous blocks for fast access.  
　　- Use CHAR instead of VARCHAR for fixed-length fields. CHAR effectively allows for fast, random access, whereas with VARCHAR, you must find the end of a string before moving onto the next one.  
　　- Use TEXT for large blocks of text such as blog posts. TEXT also allows for boolean searches. Using a TEXT field results in storing a pointer on disk that is used to locate the text block.  
　　- Use INT for larger numbers up to 2^32 or 4 billion.  
　　- Use DECIMAL for currency to avoid floating point representation errors.  
　　- VARCHAR(255) is the largest number of characters that can be counted in an 8 bit number, often maximizing the use of a byte in some RDBMS.  
　　- Set the NOT NULL constraint where applicable to improve search performance.  

　　Use good indices:  
　　- Columns that you are querying (SELECT, GROUP BY, ORDER BY, JOIN) could be faster with indices.  
　　- Indices are usually represented as self-balancing B-tree that keeps data sorted and allows searches, sequential access, insertions, and deletions in logarithmic time.  
　　- Placing an index can keep the data in memory, requiring more space.  
　　- Writes could also be slower since the index also needs to be updated.  
　　- When loading large amounts of data, it might be faster to disable indices, load the data, then rebuild the indices.  

　　Avoid expensive joins:  
　　- Denormalize where performance demands it.  

　　Partition tables:  
　　- Break up a table by putting hot spots in a separate table to help keep it in memory.  

　　Tune the query cache:  
　　- In some cases, the query cache could lead to performance issues.  

###  <font color=indigo>tips: 覃超：面试中的DFS </font>

https://www.bilibili.com/video/BV1PJ411e7ht  

##### DFS

###### DFS代码-递归写法
```py
visited = set()
def dfs(node, visited):
    visited.add(node)
    # process current node here
    for next_node in node.children():
        if not next_node in visited:
           dfs(next_node, visited)
```

###### DFS代码-非递归写法
```py
def DFS(self, tree):
    if tree.root is None:
        return []

    visited, stack = [], [tree.root]

    while stack: 
        node = stack.pop()
        visited.add(node)
        process(node)

        nodes = generate_related_nodes(node)
        stack.push(nodes)

        # other processing work
```

###  <font color=indigo>Share: boost.exception库下的命令模式</font>
　　命令模式：将请求（命令）封装为一个对象，这样可以使用不同的请求参数化其他对象（将不同请求依赖注入到其他对象），并且能够支持请求（命令）的排队执行、记录日志、撤销等（附加控制）功能。  

　　boost.exception库把错误信息封装在异常中，使用c++的异常机制传递，直到有一个catch块处理它。  

#### **【boost.exception库简介】**
　　我们先来看看boost.exception类的定义：  
```c++
    class
    BOOST_SYMBOL_VISIBLE
    exception
        {
        //<N3757>
        public:
        template <class Tag> void set( typename Tag::type const & );
        template <class Tag> typename Tag::type const * get() const;
        //</N3757>

        protected:

        exception():
            throw_function_(0),
            throw_file_(0),
            throw_line_(-1)
            {
            }

#ifdef __HP_aCC
        //On HP aCC, this protected copy constructor prevents throwing boost::exception.
        //On all other platforms, the same effect is achieved by the pure virtual destructor.
        exception( exception const & x ) BOOST_NOEXCEPT_OR_NOTHROW:
            data_(x.data_),
            throw_function_(x.throw_function_),
            throw_file_(x.throw_file_),
            throw_line_(x.throw_line_)
            {
            }
#endif

        virtual ~exception() BOOST_NOEXCEPT_OR_NOTHROW
#ifndef __HP_aCC
            = 0 //Workaround for HP aCC, =0 incorrectly leads to link errors.
#endif
            ;

#if (defined(__MWERKS__) && __MWERKS__<=0x3207) || (defined(_MSC_VER) && _MSC_VER<=1310)
        public:
#else
        private:

        template <class E>
        friend E const & exception_detail::set_info( E const &, throw_function const & );

        template <class E>
        friend E const & exception_detail::set_info( E const &, throw_file const & );

        template <class E>
        friend E const & exception_detail::set_info( E const &, throw_line const & );

        template <class E,class Tag,class T>
        friend E const & exception_detail::set_info( E const &, error_info<Tag,T> const & );

        friend char const * exception_detail::get_diagnostic_information( exception const &, char const * );

        template <class>
        friend struct exception_detail::get_info;
        friend struct exception_detail::get_info<throw_function>;
        friend struct exception_detail::get_info<throw_file>;
        friend struct exception_detail::get_info<throw_line>;
        template <class>
        friend struct exception_detail::set_info_rv;
        friend struct exception_detail::set_info_rv<throw_function>;
        friend struct exception_detail::set_info_rv<throw_file>;
        friend struct exception_detail::set_info_rv<throw_line>;
        friend void exception_detail::copy_boost_exception( exception *, exception const * );
#endif
        mutable exception_detail::refcount_ptr<exception_detail::error_info_container> data_;
        mutable char const * throw_function_;
        mutable char const * throw_file_;
        mutable int throw_line_;
        };

    inline
    exception::
    ~exception() BOOST_NOEXCEPT_OR_NOTHROW
        {
        }


**********************************
    template <class E,class Tag,class T>
    inline
    typename enable_if<exception_detail::derives_boost_exception<E>,E const &>::type
    operator<<( E const & x, error_info<Tag,T> const & v )
        {
        return exception_detail::set_info(x,v);
        }
```
　　被保护的构造函数表明了exception类要作为父类使用。要使用它必须用子类继承它。  

　　exception的重要能力在于操作符<<，它可以存储error_info对象的信息。存入的信息可以用get_error_info<>()函数随时取出来，这个函数返回一个存储数据的指针。  

　　接着看下boost.error_info类的定义：  
```c++
    template <class Tag,class T>
    class
    error_info:
        public exception_detail::error_info_base
        {
        exception_detail::error_info_base *
        clone() const
            {
            return new error_info<Tag,T>(*this);
            }
        public:
        typedef T value_type;
        error_info( value_type const & v ):
            v_(v)
            {
            }
#if (__GNUC__*100+__GNUC_MINOR__!=406) //workaround for g++ bug
#ifndef BOOST_NO_CXX11_RVALUE_REFERENCES
        error_info( error_info const & x ):
            v_(x.v_)
            {
            }
        error_info( T && v ) BOOST_NOEXCEPT_IF(boost::is_nothrow_move_constructible<T>::value):
            v_(std::move(v))
            {
            }
        error_info( error_info && x ) BOOST_NOEXCEPT_IF(boost::is_nothrow_move_constructible<T>::value):
            v_(std::move(x.v_))
            {
            }
#endif
#endif
        ~error_info() BOOST_NOEXCEPT_OR_NOTHROW
            {
            }
        value_type const &
        value() const
            {
            return v_;
            }
        value_type &
        value()
            {
            return v_;
            }
        private:
        error_info & operator=( error_info const & );
#ifndef BOOST_NO_CXX11_RVALUE_REFERENCES
        error_info & operator=( error_info && x );
#endif
        std::string name_value_string() const;
        value_type v_;
        };
    }
```
　　error_info提供了向异常类型增加信息的方法。第一个模板参数Tag是一个标记，仅用来标记error_info类。第二个模板参数T是真正存储数据的类型，可以用value()访问。  

#### **【boost.exception库的使用】**
　　示例代码如下：  
```c++
#include <boost/exception/all.hpp>
#include <exception>
#include <iostream>
#include <string>

struct MyException : virtual std::exception, virtual boost::exception {};

typedef boost::error_info<struct tag_errno, int> err_no;
typedef boost::error_info<struct tag_errstr, std::string> err_str;

int main() {
    try {
        throw MyException() << err_no(10) << err_str("error message");
    } catch (const std::exception& e) {
        std::cout << *boost::get_error_info<err_no>(e) << std::endl;
        std::cout << *boost::get_error_info<err_str>(e) << std::endl;
        std::cout << e.what() << std::endl;
    }
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 10
// error message
// std::exception
```

　　首先我们定义了一个异常类MyException，继承了std::exception和boost::exception类。使用虚继承是因为防止钻石型多重继承。  

　　接下来我们定义了异常需要存储的信息模板类error_info。第一个error_info类存储int类型的数据，第二个error_info类存储string类型的数据。  

　　发生异常时，我们使用这个自定义异常类，并用<<操作符向它存储任意信息，捕获异常时用get_error_info<>()函数提取这些信息。  


