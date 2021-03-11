---
layout: post
title: ARST(二十五)
date: 2020-10-05 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=purple>Algorithm</font>

#### **【LeetCode:36. Valid Sudoku】**

##### 　　Description:
　　Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:  

　　1.Each row must contain the digits 1-9 without repetition.  
　　2.Each column must contain the digits 1-9 without repetition.  
　　3.Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.  

##### 　　Example 1:
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
Output: true
```

##### 　　Example 2:
```sh
Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```
##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/036.ValidSudoku/ValidSudoku.cpp  

###  <font color=purple>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  

#### **【Words】**
English | Chinese
-|-
arc | n. 弧（度）


#### **【Comments and Conclusions】**

##### NoSQL
　　NoSQL is a collection of data items represented in a key-value store, document store, wide column store, or a graph database. Data is denormalized, and joins are generally done in the application code. Most NoSQL stores lack true ACID transactions and favor eventual consistency.  

　　BASE is often used to describe the properties of NoSQL databases. In comparison with the CAP Theorem, BASE chooses availability over consistency.  

　　- Basically available - the system guarantees availability.  
　　- Soft state - the state of the system may change over time, even without input.  
　　- Eventual consistency - the system will become consistent over a period of time, given that the system doesn't receive input during that period.  

##### Key-value store
　　A key-value store generally allows for O(1) reads and writes and is often backed by memory or SSD. Data stores can maintain keys in lexicographic order, allowing efficient retrieval of key ranges. Key-value stores can allow for storing of metadata with a value.  

　　Key-value stores provide high performance and are often used for simple data models or for rapidly-changing data, such as an in-memory cache layer. Since they offer only a limited set of operations, complexity is shifted to the application layer if additional operations are needed.  

　　A key-value store is the basis for more complex systems such as a document store, and in some cases, a graph database.  

##### Document store
　　A document store is centered around documents (XML, JSON, binary, etc), where a document stores all information for a given object. Document stores provide APIs or a query language to query based on the internal structure of the document itself.   

　　Based on the underlying implementation, documents are organized by collections, tags, metadata, or directories. Although documents can be organized or grouped together, documents may have fields that are completely different from each other.  

　　Some document stores like MongoDB and CouchDB also provide a SQL-like language to perform complex queries. DynamoDB supports both key-values and documents.  

　　Document stores provide high flexibility and are often used for working with occasionally changing data.  

##### Wide column store
　　A wide column store's basic unit of data is a column (name/value pair). A column can be grouped in column families (analogous to a SQL table). Super column families further group column families. You can access each column independently with a row key, and columns with the same row key form a row. Each value contains a timestamp for versioning and for conflict resolution.  

　　Google introduced Bigtable as the first wide column store, which influenced the open-source HBase often-used in the Hadoop ecosystem, and Cassandra from Facebook. Stores such as BigTable, HBase, and Cassandra maintain keys in lexicographic order, allowing efficient retrieval of selective key ranges.  

　　Wide column stores offer high availability and high scalability. They are often used for very large data sets.  

##### Graph database
　　In a graph database, each node is a record and each arc is a relationship between two nodes. Graph databases are optimized to represent complex relationships with many foreign keys or many-to-many relationships.  

　　Graphs databases offer high performance for data models with complex relationships, such as a social network. They are relatively new and are not yet widely-used; it might be more difficult to find development tools and resources. Many graphs can only be accessed with REST APIs.  
　
###  <font color=purple>tips: 覃超：技术人的职业规划</font>
https://www.bilibili.com/video/BV1d7411G7xe  
　　摒弃学生思维：  
　　1. Proactive - 主动揽活和思考  
　　2. Prioritize tasks - 去做impact和urgency最高的事情  
　　3. 拒绝简单任务的诱惑  

　　1-3年进阶时期  
　　1. 某一块业务的authority  
　　2. 培养人、建立团队（Productivity & proactive -> more scope -> need more resources -> bigger team -> leadership）  
　　3. 从公司全局、老板角度，思考团队和项目的发展  

###  <font color=purple>Share: boost.property_tree库下的组合模式</font>
　　组合模式：将一组对象组织（Compose）成树形结构，以表示一种“部分 - 整体”的层次结构。组合让客户端可以统一单个对象和组合对象的处理逻辑。  

　　在property_tree中，属性树的每一个子节点也是属性树，属性树可以有任意复杂的组合，但是最终呈现给用户的还是一个basic_ptree接口，使用时完全不需要关心它内部的复杂结构。  

#### **【boost.property_tree库简介】**
　　boost.property_tree库的核心类是basic_ptree，它的类定义如下：  

```c++
    /**
     * Property tree main structure. A property tree is a hierarchical data
     * structure which has one element of type @p Data in each node, as well
     * as an ordered sequence of sub-nodes, which are additionally identified
     * by a non-unique key of type @p Key.
     *
     * Key equivalency is defined by @p KeyCompare, a predicate defining a
     * strict weak ordering.
     *
     * Property tree defines a Container-like interface to the (key-node) pairs
     * of its direct sub-nodes. The iterators are bidirectional. The sequence
     * of nodes is held in insertion order, not key order.
     */
    template<class Key, class Data, class KeyCompare>
    class basic_ptree
    {
#if defined(BOOST_PROPERTY_TREE_DOXYGEN_INVOKED)
    public:
#endif
        // Internal types
        /**
         * Simpler way to refer to this basic_ptree\<C,K,P,A\> type.
         * Note that this is private, and made public only for doxygen.
         */
        typedef basic_ptree<Key, Data, KeyCompare> self_type;

        // Container view types
        typedef std::pair<const Key, self_type>      value_type;
...
...


        /** Creates a node with no children and default-constructed data. */
        basic_ptree();
        /** Creates a node with no children and a copy of the given data. */
        explicit basic_ptree(const data_type &data);
        basic_ptree(const self_type &rhs);
        ~basic_ptree();
        /** Basic guarantee only. */
        self_type &operator =(const self_type &rhs);

        /** Swap with other tree. Only constant-time and nothrow if the
         * data type's swap is.
         */
        void swap(self_type &rhs);

        // Container view functions

        /** The number of direct children of this node. */
        size_type size() const;
        size_type max_size() const;
        /** Whether there are any direct children. */
        bool empty() const;

        iterator begin();
        iterator end();

        value_type &front();
        value_type &back();

        /** Insert a copy of the given tree with its key just before the given
         * position in this node. This operation invalidates no iterators.
         * @return An iterator to the newly created child.
         */
        iterator insert(iterator where, const value_type &value);

        /** Range insert. Equivalent to:
         * @code
         * for(; first != last; ++first) insert(where, *first);
         * @endcode
         */
        template<class It> void insert(iterator where, It first, It last);

        /** Erase the child pointed at by the iterator. This operation
         * invalidates the given iterator, as well as its equivalent
         * assoc_iterator.
         * @return A valid iterator pointing to the element after the erased.
         */
        iterator erase(iterator where);

        /** Range erase. Equivalent to:
         * @code
         * while(first != last;) first = erase(first);
         * @endcode
         */
        iterator erase(iterator first, iterator last);

        /** Equivalent to insert(begin(), value). */
        iterator push_front(const value_type &value);

        /** Equivalent to insert(end(), value). */
        iterator push_back(const value_type &value);

        /** Equivalent to erase(begin()). */
        void pop_front();

        /** Equivalent to erase(boost::prior(end())). */
        void pop_back();

        /** Reverses the order of direct children in the property tree. */
        void reverse();

        /** Sorts the direct children of this node according to the predicate.
         * The predicate is passed the whole pair of key and child.
         */
        template<class Compare> void sort(Compare comp);

        /** Sorts the direct children of this node according to key order. */
        void sort();

        /** Reference to the actual data in this node. */
        data_type &data();

        /** Clear this tree completely, of both data and children. */
        void clear();

        /** Get the child at the given path, or throw @c ptree_bad_path.
         * @note Depending on the path, the result at each level may not be
         *       completely deterministic, i.e. if the same key appears multiple
         *       times, which child is chosen is not specified. This can lead
         *       to the path not being resolved even though there is a
         *       descendant with this path. Example:
         * @code
         *   a -> b -> c
         *     -> b
         * @endcode
         *       The path "a.b.c" will succeed if the resolution of "b" chooses
         *       the first such node, but fail if it chooses the second.
         */
        self_type &get_child(const path_type &path);

        /** Set the node at the given path to the given value. Create any
         * missing parents. If the node at the path already exists, replace it.
         * @return A reference to the inserted subtree.
         * @note Because of the way paths work, it is not generally guaranteed
         *       that a node newly created can be accessed using the same path.
         * @note If the path could refer to multiple nodes, it is unspecified
         *       which one gets replaced.
         */
        self_type &put_child(const path_type &path, const self_type &value);

        /** Add the node at the given path. Create any missing parents. If there
         * already is a node at the path, add another one with the same key.
         * @param path Path to the child. The last fragment must not have an
         *             index.
         * @return A reference to the inserted subtree.
         * @note Because of the way paths work, it is not generally guaranteed
         *       that a node newly created can be accessed using the same path.
         */
        self_type &add_child(const path_type &path, const self_type &value);
    };
```

　　basic_ptree的接口很像标准容器std::list，可以执行很多基本的元素操作，如使用begin()和end()遍历当前属性树的所有子节点。  

　　basic_ptree类有两个重要的内部类型定义self_type和value_type。self_type是basic_ptree模板实例化后自身的类型，它也是子节点的类型。value_type是节点的数据结构，它是一个std::pair，包含属性名和节点自身。这两个类型定义意味着basic_ptree使用了组合模式，它是有多个basic_ptree组合而成的集合体。  

　　接下来我们主要使用ptree，它的定义是：  
```c++
    typedef basic_ptree<std::string, std::string> ptree;
```

#### **【boost.property_tree库的使用】**
　　示例代码如下：  
```c++
#include <boost/property_tree/ptree.hpp>
#include <boost/property_tree/xml_parser.hpp>
#include <boost/typeof/typeof.hpp>
#include <iostream>
#include <string>

int main() {
    boost::property_tree::ptree pt;
    boost::property_tree::read_xml("conf.xml", pt);

    std::cout << pt.get<std::string>("conf.theme") << std::endl;
    std::cout << pt.get<int>("conf.gui") << std::endl;
    std::cout << pt.get<int>("conf.style") << std::endl;
    std::cout << pt.get("conf.no_prop", 100) << std::endl;

    BOOST_AUTO(child, pt.get_child("conf.urls"));
    for (BOOST_AUTO(pos, child.begin()); pos != child.end(); pos++) {
        std::cout << pos->first << "->" << pos->second.data() << std::endl;
    }
    return 0;
}
```
　　ptree通过模板成员函数get()通过路径访问属性树内的节点，用模板参数指明获取属性的类型。  

　　property_tree的路径语法很自然很容易理解，使用点号作为路径的分隔符，如：  
```c++
    std::cout << pt.get<std::string>("conf.theme") << std::endl;
```
　　对于有多个子节点的节点，可以使用get_child()先获得子节点对象，然后使用迭代器遍历子节点。  