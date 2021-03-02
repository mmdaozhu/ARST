---
layout: post
title: ARST(三十三)
date: 2020-11-30 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=orange>Algorithm</font>

#### **【LeetCode:51. N-Queens】**

##### 　　Description:
　　The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.  

　　Given an integer n, return all distinct solutions to the n-queens puzzle.  

　　Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.  

##### 　　Example 1:
![hello](/ARTS-33/1.jpg)  
```sh
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```

##### 　　Example 2:
```sh
Input: n = 1
Output: [["Q"]]
```

##### 　　C++ Solution 1:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/051.N-Queens/N-Queens1.cpp  

##### 　　C++ Solution 2:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/051.N-Queens/N-Queens2.cpp  

##### 　　C++ Solution 3:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/051.N-Queens/N-Queens3.cpp  

###  <font color=orange>Review</font>

#### **【Article】**
https://www.modernescpp.com/index.php/c-20-an-overview  
　　The Next Big Thing: C++20  

#### **【Words】**
English | Chinese
-|-
Semantic | adj. 语义的；语义学的
emphasise | vt. （英）强调
variadic template | 可变参数模板

#### **【Comments and Conclusions】**

##### historical context of C++ standards
![hello](/ARTS-33/2.png)  

##### C++98
　　C++98 had a few important features: templates, the standard template library (STL) with its containers and algorithms, strings, and IO streams.  

##### C++03
　　With C++03 (14882:2003), C++98 got a technical correction.  

##### TR1
　　TR1 was a big step toward C++11 and, therefore, towards Modern C++. TR1 (TR 19768) is based on the boost project which was founded by members of the C++ standardisation committee. TR1 has 13 libraries that should become part of the next C++ standard.  

##### C++11
　　C++11 stands for the next C++ standard but we just call it Modern C++. C++11 has many features which fundamentally changed the way we program C++.  

　　For example, C++11 brought the components of the TR1, but also move-semantic, perfect forwarding, variadic templates, or constexpr. But this not all. With C++11 we got the first time a memory model as the fundamental base of threading and a threading API.  

##### C++14
　　C++14 is a small C++ standard. It brought read-writer locks, generalised lambdas, and generalised constexpr functions.  

##### C++17
　　It has two outstanding features: the parallel STL and the standardised filesystem. We got the filesystem from boost and the three new data types: std::optional, std::variant, and std::any.  

##### C++20
　　C++20 will change the way we program C++ as fundamentally as C++11. This holds is, in particular, true for the big four: Ranges, Coroutines, Concepts, and Modules.  

　　The new ranges library enables it to express algorithms directly on the container, compose the algorithm with the pipe symbol and apply them onto infinite data streams.  

　　Thanks to Coroutines asynchronous programming in C++ may become mainstream. Coroutines are the base for cooperative tasks, event loops, infinite data streams, or pipelines.  

　　Concepts will change the way we think and program templates. They are semantic categories for the valid template arguments. They enable you to express your intention directly in the type system. If something goes wrong, you will get a short error message.  

　　Modules will overcome the restrictions of header files. They promise a lot. For example, the separation of header and source files becomes as obsolete as the preprocessor. In the end, we will also have faster build times and an easier way to build packages.  
　
###  <font color=orange>tips: two-phase commit(2PC)</font>
　　two-phase commit: An operation that is part of a distributed transaction, under the XA specification.  

#### **【MySQL two-phase commit】**
　　When committing a transaction, MySQL employs a two-phase commit protocol.  

　　Example: MySQL executes a update SQL statement.  

　　First phase: MySQL storage engine updates new data into the memory and records the update operation into the redo log. The redo log is being in the prepare state and commits the transaction at any time.  

　　Next step: MySQL executors records the update operation into the bin log and writes the bin log into the disks.  

　　Second phase: MySQL storage engine changes the redo log state from prepare state to commit state. At this time, the update operation is finished.  

###  <font color=orange>Share: 高性能数据库集群：分库分表</font>

#### **【业务分库】**
　　<b>业务分库指的是按照业务模块将数据分散到不同的数据库服务器</b>下面是其基本架构图。  
![hello](/ARTS-33/3.jpg)

#### **【业务分库带来的问题】**

##### 1. join 操作
　　问题业务分库后，原本在同一个数据库中的表分散到不同数据库中，导致无法使用 SQL 的 join 查询。  

　　解决方法：多次查询相关数据库，手动join。  

##### 2. 事务问题
　　原本在同一个数据库中不同的表可以在同一个事务中修改，业务分库后，表分散到不同的数据库中，无法通过事务统一修改。  

　　解决方法：需要业务程序自己来模拟实现事务的功能。  

##### 3. 成本问题
　　业务分库同时也带来了成本的代价，本来 1 台服务器搞定的事情，现在要 3 台，如果考虑备份，那就是 2 台变成了 6 台。  

#### **【分表】**
　　单表数据拆分有两种方式：<b>垂直分表和水平分表</b>。示意图如下：  
![hello](/ARTS-33/4.jpg)  

#### **【垂直分表】**
　　垂直分表适合将表中某些不常用且占了大量空间的列拆分出去。  

　　垂直分表引入的复杂性主要体现在表操作的数量要增加。  

#### **【垂直分表】**
　　水平分表适合表行数特别大的表。  

　　水平分表相比垂直分表，会引入更多的复杂性。  

##### 1. 路由
　　水平分表后，某条数据具体属于哪个切分后的子表，需要增加路由算法进行计算，这个算法会引入一定的复杂性。  

　　<b>范围路由</b>：选取有序的数据列（例如，整形、时间戳等）作为路由的条件，不同分段分散到不同的数据库表中。范围路由的优点是可以随着数据的增加平滑地扩充新的表。范围路由的一个比较隐含的缺点是分布不均匀。  

　　<b>Hash 路由</b>：选取某个列（或者某几个列组合也可以）的值进行 Hash 运算，然后根据 Hash 结果分散到不同的数据库表中。Hash 路由的优缺点和范围路由基本相反，Hash 路由的优点是表分布比较均匀，缺点是扩充新的表很麻烦，所有数据都要重分布。  

　　<b>配置路由</b>：配置路由就是路由表，用一张独立的表来记录路由信息。配置路由设计简单，使用起来非常灵活，尤其是在扩充表的时候，只需要迁移指定的数据，然后修改路由表就可以了。配置路由的缺点就是必须多查询一次，会影响整体性能。  

##### 2. join 操作
　　水平分表后，数据分散在多个表中，如果需要与其他表进行 join 查询，需要在业务代码或者数据库中间件中进行多次 join 查询，然后将结果合并。  

##### 3. count() 操作
　　<b>count() 相加</b>：具体做法是在业务代码或者数据库中间件中对每个表进行 count() 操作，然后将结果相加。这种方式实现简单，缺点就是性能比较低。  

　　<b>记录数表</b>：具体做法是新建一张表，假如表名为“记录数表”，包含 table_name、row_count 两个字段，每次插入或者删除子表数据成功后，都更新“记录数表”。优点是只需要一次简单查询就可以获取数据。缺点是复杂度增加不少，对子表的操作要同步操作“记录数表”。  

　　对于一些不要求记录数实时保持精确的业务，也可以通过后台定时更新记录数表。定时更新实际上就是“count() 相加”和“记录数表”的结合，即定时通过 count() 相加计算表的记录数，然后更新记录数表中的数据。  

##### 3. order by 操作
　　水平分表后，数据分散到多个子表中，排序操作无法在数据库中完成，只能由业务代码或者数据库中间件分别查询每个子表中的数据，然后汇总进行排序。  

#### **【实现方法】**
　　分库分表具体的实现方式也是“程序代码封装”和“中间件封装”，但实现会更复杂。  

　　分库分表的实现除了要判断操作类型外，还要判断 SQL 中具体需要操作的表、操作函数（例如 count 函数)、order by、group by 操作等，然后再根据不同的操作进行不同的处理。  

#### **【脑图】**
![hello](/ARTS-33/分库分表.png)  

#### **【思考题】**
　　你认为什么时候引入分库分表是合适的？是数据库性能不够的时候就开始分库分表么？  

　　首先采取SQL优化、引入缓存等方式。  