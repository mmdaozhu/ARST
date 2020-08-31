---
layout: post
title: ARST(十五)
date: 2020-07-27 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=green>Algorithm</font>

#### **【LeetCode:104. Maximum Depth of Binary Tree】**

##### 　　Description:
　　Given a binary tree, find its maximum depth.  
　　The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.  

##### 　　Example 1:
```sh
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7

return its depth = 3.
```

##### 　　Note:
　　A leaf is a node with no children.  

##### 　　C++ Solution:
```cpp
#include <cassert>
#include <iostream>

/*
解体思路：
    递归

时间复杂度分析：O(n)
*/

/**
 * Definition for a binary tree node.
 *
 */
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    int maxDepth(TreeNode *root) {
        if (root == nullptr) {
            return 0;
        }

        //reduce recursion times.
        if (root->left == nullptr && root->right == nullptr) {
            return 1;
        }

        int left = maxDepth(root->left);
        int right = maxDepth(root->right);
        return left > right ? left + 1 : right + 1;
    }
};
```

###  <font color=green>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  
https://snarfed.org/transactions_across_datacenters_io.html  

#### **【Comments and Conclusions】**
　　- Weak consistency: After a write, reads may or may not see it. A best effort approach is taken.  
　　App Engine: memcache such as VoIP, video chat, and realtime multiplayer games.  

　　- Eventual consistency: After a write, reads eventually see it. Data is replicated asynchronously.  
　　App Engine: DNS, SMTP, snail mail  

　　- Strong consistency: After a write, reads will see it. Data is replicated synchronously.  
　　App Engine: datastore such as file systems, RDBMSec.  

###  tips

###  <font color=green>Share: boost.array库下的适配器模式</font>
　　首先了解一下适配器模式。  
　　这个模式就是用来做适配的，它将不兼容的接口转换为可兼容的接口，让原本由于接口不兼容而不能一起工作的类可以一起工作。对于这个模式，有一个经常被拿来解释它的例子，就是USB转接头充当适配器，把两种不兼容的接口，通过转接变得可以一起工作。  

#### **【array库简介】**
　　array包装了c++语言内建的数组，为其提供标准的STL容器接口，如begin()、front()等。性能上与原始数组相差无几，在程序对性能要求很高，或者不需要动态数组的情况下可以使用。  

#### **【array库用法】**
```c++
#include <algorithm>
#include <boost/array.hpp>
#include <boost/typeof/typeof.hpp>
#include <cassert>
#include <iostream>

int main() {
    boost::array<int, 10> ar;
    ar[0] = 1;
    ar.back() = 10;
    std::cout << "size: " << ar.max_size() << std::endl;
    ar.assign(777);
    for (BOOST_AUTO(pos, ar.begin()); pos != ar.end(); ++pos) {
        std::cout << *pos << ",";
    }
    std::cout << std::endl;
    int *p = ar.c_array();
    *(p + 5) = 253;
    std::cout << ar[5] << std::endl;
    ar.at(8) = 666;
    std::sort(ar.begin(), ar.end());
    for (BOOST_AUTO(pos, ar.begin()); pos != ar.end(); ++pos) {
        std::cout << *pos << ",";
    }
    std::cout << std::endl;
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// size: 10
// 777,777,777,777,777,777,777,777,777,777,
// 253
// 253,666,777,777,777,777,777,777,777,777,
```

#### **【array库源码剖析】**
　　直接看源码：  
```c++
template<class T, std::size_t N>
    class array {
      public:
        T elems[N];    // fixed-size array of elements of type T

      public:
        // type definitions
        typedef T              value_type;
        typedef T*             iterator;
        typedef const T*       const_iterator;
        typedef T&             reference;
        typedef const T&       const_reference;
        typedef std::size_t    size_type;
        typedef std::ptrdiff_t difference_type;

        // iterator support
        iterator        begin()       { return elems; }
        const_iterator  begin() const { return elems; }
        const_iterator cbegin() const { return elems; }
        
        iterator        end()       { return elems+N; }
        const_iterator  end() const { return elems+N; }
        const_iterator cend() const { return elems+N; }

        // reverse iterator support
#if !defined(BOOST_MSVC_STD_ITERATOR) && !defined(BOOST_NO_STD_ITERATOR_TRAITS)
        typedef std::reverse_iterator<iterator> reverse_iterator;
        typedef std::reverse_iterator<const_iterator> const_reverse_iterator;
#elif defined(_RWSTD_NO_CLASS_PARTIAL_SPEC) 
        typedef std::reverse_iterator<iterator, std::random_access_iterator_tag, 
              value_type, reference, iterator, difference_type> reverse_iterator; 
        typedef std::reverse_iterator<const_iterator, std::random_access_iterator_tag,
              value_type, const_reference, const_iterator, difference_type> const_reverse_iterator;
#else
        // workaround for broken reverse_iterator implementations
        typedef std::reverse_iterator<iterator,T> reverse_iterator;
        typedef std::reverse_iterator<const_iterator,T> const_reverse_iterator;
#endif

        reverse_iterator rbegin() { return reverse_iterator(end()); }
        const_reverse_iterator rbegin() const {
            return const_reverse_iterator(end());
        }
        const_reverse_iterator crbegin() const {
            return const_reverse_iterator(end());
        }

        reverse_iterator rend() { return reverse_iterator(begin()); }
        const_reverse_iterator rend() const {
            return const_reverse_iterator(begin());
        }
        const_reverse_iterator crend() const {
            return const_reverse_iterator(begin());
        }

        // operator[]
        reference operator[](size_type i) 
        { 
            return BOOST_ASSERT_MSG( i < N, "out of range" ), elems[i]; 
        }
        
        /*BOOST_CONSTEXPR*/ const_reference operator[](size_type i) const 
        {     
            return BOOST_ASSERT_MSG( i < N, "out of range" ), elems[i]; 
        }

        // at() with range check
        reference                           at(size_type i)       { return rangecheck(i), elems[i]; }
        /*BOOST_CONSTEXPR*/ const_reference at(size_type i) const { return rangecheck(i), elems[i]; }
    
        // front() and back()
        reference front() 
        { 
            return elems[0]; 
        }
        
        BOOST_CONSTEXPR const_reference front() const 
        {
            return elems[0];
        }
        
        reference back() 
        { 
            return elems[N-1]; 
        }
        
        BOOST_CONSTEXPR const_reference back() const 
        { 
            return elems[N-1]; 
        }

        // size is constant
        static BOOST_CONSTEXPR size_type size() { return N; }
        static BOOST_CONSTEXPR bool empty() { return false; }
        static BOOST_CONSTEXPR size_type max_size() { return N; }
        enum { static_size = N };

        // swap (note: linear complexity)
        void swap (array<T,N>& y) {
            for (size_type i = 0; i < N; ++i)
                boost::swap(elems[i],y.elems[i]);
        }

        // direct access to data (read-only)
        const T* data() const { return elems; }
        T* data() { return elems; }

        // use array as C array (direct read/write access to data)
        T* c_array() { return elems; }

        // assignment with type conversion
        template <typename T2>
        array<T,N>& operator= (const array<T2,N>& rhs) {
            std::copy(rhs.begin(),rhs.end(), begin());
            return *this;
        }

        // assign one value to all elements
        void assign (const T& value) { fill ( value ); }    // A synonym for fill
        void fill   (const T& value)
        {
            std::fill_n(begin(),size(),value);
        }

        // check range (may be private because it is static)
        static BOOST_CONSTEXPR bool rangecheck (size_type i) {
            return i >= size() ? boost::throw_exception(std::out_of_range ("array<>: index out of range")), true : true;
        }

    };
```
　　array库采用了适配器模式，将普通数组的接口进行了包装适配，使得可以与标准算法一起工作。  


