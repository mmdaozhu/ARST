---
layout: post
title: ARST(十二)
date: 2020-07-06 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=yellow>Algorithm</font>

#### **【LeetCode:344. Reverse String】**

##### 　　Description:
　　Write a function that reverses a string. The input string is given as an array of characters char[].  
　　Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.  
　　You may assume all the characters consist of printable ascii characters.  

##### 　　Example 1:
```sh
Input: ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

##### 　　Example 2:
```sh
Input: ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

##### 　　C++ Solution:
```cpp
#include <assert.h>

#include <iostream>
#include <vector>

/*
解体思路：
    因为vector是顺序存储的，做reverse直接用索引替换就好了。

时间复杂度分析：O(n)
*/

class Solution {
public:
    void reverseString(std::vector<char>& s) {
        for (size_t i = 0; i < s.size() / 2; i++) {
            std::swap(s[i], s[s.size() - i - 1]);
        }
    }
};
```

###  <font color=yellow>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer  
https://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html  
https://www.slideshare.net/jboner/scalability-availability-stability-patterns/  


#### **【Words】**
English | Chinese
-|-
proportional | adj. 比例的，成比例的；相称的，均衡的
incantation | n. 咒语


#### **【Comments and Conclusions】**
　　A service is said to be scalable if when we increase the resources in a system, it results in increased performance in a manner proportional to resources added. Increasing performance in general means serving more units of work, but it can also be to handle larger units of work, such as when datasets grow.  
　　Another way to look at performance vs scalability.  
　　If you have a performance problem, your system is slow for a single user.  
　　If you have a scalability problem, your system is fast for a single user but slow under heavy load.  

###  tips

###  <font color=yellow>Share: smart_ptr 下的代理模式</font>
　　首先了解一下代理模式。  
　　它在不改变原始类（或叫被代理类）代码的情况下，通过引入代理类来给原始类附加功能。  
　　smart_ptr实践了代理模式，代理了原始“裸”指针的行为，为它增加了更多更有用的特性。  

#### **【shared_ptr简介】**
　　shared_ptr是一个最像指针的“智能指针”，是boost.smart_ptr库中最有价值、最为重要的组成部分，也是最有用的。  
　　shared_ptr实现的是引用计数型的智能指针，可以被自由地拷贝和赋值，在任意对方共享它，当没有代码使用它时才删除被包装的动态分配的对象。  
　　shared_ptr也可以安全地放到标准容器中。  

#### **【shared_ptr用法】**
```c++
#include <boost/shared_ptr.hpp>
#include <iostream>

class shared {
public:
    shared(boost::shared_ptr<int> p) : p(p) {}
    void print() { std::cout << "count: " << p.use_count() << ", v= " << *p << std::endl; }

private:
    boost::shared_ptr<int> p;
};

void print_func(boost::shared_ptr<int> p) { std::cout << "count: " << p.use_count() << ", v= " << *p << std::endl; }

int main() {
    boost::shared_ptr<int> p(new int(100));
    shared s1(p);
    shared s2(p);
    s1.print();
    s2.print();
    *p = 20;
    print_func(p);
    s1.print();
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// count: 3, v= 100
// count: 3, v= 100
// count: 3, v= 20
// count: 3, v= 20
```

#### **【shared_ptr的工厂函数】**
　　shared_ptr很好地消除了显式地delete调用，但是这还不够，因为shared_ptr的构造还需要new调用，这导致了代码中的某种不对称性。这个时候应该用工厂模式解决。  
　　make_shared()函数要比直接创建shared_ptr的对象的方式快且高效，因为它内部仅分配一次内存，消除了shared_ptr构造时的开销。  
```c++
#include <boost/make_shared.hpp>
#include <boost/shared_ptr.hpp>
#include <iostream>
#include <string>
#include <vector>

int main() {
    boost::shared_ptr<std::string> sp = boost::make_shared<std::string>("make_shared");
    std::cout << sp->size() << std::endl;
    boost::shared_ptr<std::vector<int>> spv = boost::make_shared<std::vector<int>>(10, 2);
    std::cout << spv->size() << std::endl;
    return 0;
}
// g++ -std=c++11 -Wall -I /var/boost_1_73_0/include/ test.cpp -o test
// result:
// 11
// 10
```

#### **【shared_ptr应用于标准容器】**
```c++
#include <boost/make_shared.hpp>
#include <boost/shared_ptr.hpp>
#include <iostream>
#include <vector>

int main() {
    std::vector<boost::shared_ptr<int>> vs(10);

    int i = 0;
    for (auto pos = vs.begin(); pos != vs.end(); ++pos) {
        *pos = boost::make_shared<int>(++i);
        std::cout << *(*pos) << std::endl;
    }
    boost::shared_ptr<int> p = vs[9];
    *p = 100;
    std::cout << *vs[9] << std::endl;
    return 0;
}
```

#### **【shared_ptr源码剖析】**
```c++
//
//  shared_ptr
//
//  An enhanced relative of scoped_ptr with reference counted copy semantics.
//  The object pointed to is deleted when the last shared_ptr pointing to it
//  is destroyed or reset.
//

template<class T> class shared_ptr
{
private:

    // Borland 5.5.1 specific workaround
    typedef shared_ptr<T> this_type;

public:

    typedef typename boost::detail::sp_element< T >::type element_type;

    BOOST_CONSTEXPR shared_ptr() BOOST_SP_NOEXCEPT : px( 0 ), pn()
    {
    }

#if !defined( BOOST_NO_CXX11_NULLPTR )

    BOOST_CONSTEXPR shared_ptr( boost::detail::sp_nullptr_t ) BOOST_SP_NOEXCEPT : px( 0 ), pn()
    {
    }

#endif

// ..........................................................................

    template<class Y>
    explicit shared_ptr( Y * p ): px( p ), pn() // Y must be complete
    {
        boost::detail::sp_pointer_construct( this, p, pn );
    }

    //
    // Requirements: D's copy constructor must not throw
    //
    // shared_ptr will release p by calling d(p)
    //

    template<class Y, class D> shared_ptr( Y * p, D d ): px( p ), pn( p, d )
    {
        boost::detail::sp_deleter_construct( this, p );
    }

#if !defined( BOOST_NO_CXX11_NULLPTR )

    template<class D> shared_ptr( boost::detail::sp_nullptr_t p, D d ): px( p ), pn( p, d )
    {
    }

#endif

    // As above, but with allocator. A's copy constructor shall not throw.

    template<class Y, class D, class A> shared_ptr( Y * p, D d, A a ): px( p ), pn( p, d, a )
    {
        boost::detail::sp_deleter_construct( this, p );
    }

// ..........................................................................

//  generated copy constructor, destructor are fine...

#if !defined( BOOST_NO_CXX11_RVALUE_REFERENCES )

// ... except in C++0x, move disables the implicit copy

    shared_ptr( shared_ptr const & r ) BOOST_SP_NOEXCEPT : px( r.px ), pn( r.pn )
    {
    }

#endif

    template<class Y>
    explicit shared_ptr( weak_ptr<Y> const & r ): pn( r.pn ) // may throw
    {
        boost::detail::sp_assert_convertible< Y, T >();

        // it is now safe to copy r.px, as pn(r.pn) did not throw
        px = r.px;
    }

    template<class Y>
    shared_ptr( weak_ptr<Y> const & r, boost::detail::sp_nothrow_tag )
    BOOST_SP_NOEXCEPT : px( 0 ), pn( r.pn, boost::detail::sp_nothrow_tag() )
    {
        if( !pn.empty() )
        {
            px = r.px;
        }
    }

    template<class Y>
#if !defined( BOOST_SP_NO_SP_CONVERTIBLE )

    shared_ptr( shared_ptr<Y> const & r, typename boost::detail::sp_enable_if_convertible<Y,T>::type = boost::detail::sp_empty() )

#else

    shared_ptr( shared_ptr<Y> const & r )

#endif
    BOOST_SP_NOEXCEPT : px( r.px ), pn( r.pn )
    {
        boost::detail::sp_assert_convertible< Y, T >();
    }

    // aliasing
    template< class Y >
    shared_ptr( shared_ptr<Y> const & r, element_type * p ) BOOST_SP_NOEXCEPT : px( p ), pn( r.pn )
    {
    }

#ifndef BOOST_NO_AUTO_PTR

    template<class Y>
    explicit shared_ptr( std::auto_ptr<Y> & r ): px(r.get()), pn()
    {
        boost::detail::sp_assert_convertible< Y, T >();

        Y * tmp = r.get();
        pn = boost::detail::shared_count( r );

        boost::detail::sp_deleter_construct( this, tmp );
    }

#if !defined( BOOST_NO_CXX11_RVALUE_REFERENCES )

    template<class Y>
    shared_ptr( std::auto_ptr<Y> && r ): px(r.get()), pn()
    {
        boost::detail::sp_assert_convertible< Y, T >();

        Y * tmp = r.get();
        pn = boost::detail::shared_count( r );

        boost::detail::sp_deleter_construct( this, tmp );
    }

#elif !defined( BOOST_NO_SFINAE ) && !defined( BOOST_NO_TEMPLATE_PARTIAL_SPECIALIZATION )

    template<class Ap>
    explicit shared_ptr( Ap r, typename boost::detail::sp_enable_if_auto_ptr<Ap, int>::type = 0 ): px( r.get() ), pn()
    {
        typedef typename Ap::element_type Y;

        boost::detail::sp_assert_convertible< Y, T >();

        Y * tmp = r.get();
        pn = boost::detail::shared_count( r );

        boost::detail::sp_deleter_construct( this, tmp );
    }

#endif // BOOST_NO_SFINAE, BOOST_NO_TEMPLATE_PARTIAL_SPECIALIZATION

#endif // BOOST_NO_AUTO_PTR

#if !defined( BOOST_NO_CXX11_SMART_PTR ) && !defined( BOOST_NO_CXX11_RVALUE_REFERENCES )

    template< class Y, class D >
    shared_ptr( std::unique_ptr< Y, D > && r ): px( r.get() ), pn()
    {
        boost::detail::sp_assert_convertible< Y, T >();

        typename std::unique_ptr< Y, D >::pointer tmp = r.get();

        if( tmp != 0 )
        {
            pn = boost::detail::shared_count( r );
            boost::detail::sp_deleter_construct( this, tmp );
        }
    }

#endif

    template< class Y, class D >
    shared_ptr( boost::movelib::unique_ptr< Y, D > r ): px( r.get() ), pn()
    {
        boost::detail::sp_assert_convertible< Y, T >();

        typename boost::movelib::unique_ptr< Y, D >::pointer tmp = r.get();

        if( tmp != 0 )
        {
            pn = boost::detail::shared_count( r );
            boost::detail::sp_deleter_construct( this, tmp );
        }
    }

    // assignment

    shared_ptr & operator=( shared_ptr const & r ) BOOST_SP_NOEXCEPT
    {
        this_type(r).swap(*this);
        return *this;
    }

#if !defined(BOOST_MSVC) || (BOOST_MSVC >= 1400)

    template<class Y>
    shared_ptr & operator=(shared_ptr<Y> const & r) BOOST_SP_NOEXCEPT
    {
        this_type(r).swap(*this);
        return *this;
    }

#endif

#ifndef BOOST_NO_AUTO_PTR

    template<class Y>
    shared_ptr & operator=( std::auto_ptr<Y> & r )
    {
        this_type( r ).swap( *this );
        return *this;
    }

#if !defined( BOOST_NO_CXX11_RVALUE_REFERENCES )

    template<class Y>
    shared_ptr & operator=( std::auto_ptr<Y> && r )
    {
        this_type( static_cast< std::auto_ptr<Y> && >( r ) ).swap( *this );
        return *this;
    }

#elif !defined( BOOST_NO_SFINAE ) && !defined( BOOST_NO_TEMPLATE_PARTIAL_SPECIALIZATION )

    template<class Ap>
    typename boost::detail::sp_enable_if_auto_ptr< Ap, shared_ptr & >::type operator=( Ap r )
    {
        this_type( r ).swap( *this );
        return *this;
    }

#endif // BOOST_NO_SFINAE, BOOST_NO_TEMPLATE_PARTIAL_SPECIALIZATION

#endif // BOOST_NO_AUTO_PTR

#if !defined( BOOST_NO_CXX11_SMART_PTR ) && !defined( BOOST_NO_CXX11_RVALUE_REFERENCES )

    template<class Y, class D>
    shared_ptr & operator=( std::unique_ptr<Y, D> && r )
    {
        this_type( static_cast< std::unique_ptr<Y, D> && >( r ) ).swap(*this);
        return *this;
    }

#endif

    template<class Y, class D>
    shared_ptr & operator=( boost::movelib::unique_ptr<Y, D> r )
    {
        // this_type( static_cast< unique_ptr<Y, D> && >( r ) ).swap( *this );

        boost::detail::sp_assert_convertible< Y, T >();

        typename boost::movelib::unique_ptr< Y, D >::pointer p = r.get();

        shared_ptr tmp;

        if( p != 0 )
        {
            tmp.px = p;
            tmp.pn = boost::detail::shared_count( r );

            boost::detail::sp_deleter_construct( &tmp, p );
        }

        tmp.swap( *this );

        return *this;
    }

// Move support
// ..........................................................................

    void reset() BOOST_SP_NOEXCEPT
    {
        this_type().swap(*this);
    }

    template<class Y> void reset( Y * p ) // Y must be complete
    {
        BOOST_ASSERT( p == 0 || p != px ); // catch self-reset errors
        this_type( p ).swap( *this );
    }

    template<class Y, class D> void reset( Y * p, D d )
    {
        this_type( p, d ).swap( *this );
    }

    template<class Y, class D, class A> void reset( Y * p, D d, A a )
    {
        this_type( p, d, a ).swap( *this );
    }

    template<class Y> void reset( shared_ptr<Y> const & r, element_type * p ) BOOST_SP_NOEXCEPT
    {
        this_type( r, p ).swap( *this );
    }

#if !defined( BOOST_NO_CXX11_RVALUE_REFERENCES )

    template<class Y> void reset( shared_ptr<Y> && r, element_type * p ) BOOST_SP_NOEXCEPT
    {
        this_type( static_cast< shared_ptr<Y> && >( r ), p ).swap( *this );
    }

#endif

    typename boost::detail::sp_dereference< T >::type operator* () const BOOST_SP_NOEXCEPT_WITH_ASSERT
    {
        BOOST_ASSERT( px != 0 );
        return *px;
    }
    
    typename boost::detail::sp_member_access< T >::type operator-> () const BOOST_SP_NOEXCEPT_WITH_ASSERT
    {
        BOOST_ASSERT( px != 0 );
        return px;
    }
    
    typename boost::detail::sp_array_access< T >::type operator[] ( std::ptrdiff_t i ) const BOOST_SP_NOEXCEPT_WITH_ASSERT
    {
        BOOST_ASSERT( px != 0 );
        BOOST_ASSERT( i >= 0 && ( i < boost::detail::sp_extent< T >::value || boost::detail::sp_extent< T >::value == 0 ) );

        return static_cast< typename boost::detail::sp_array_access< T >::type >( px[ i ] );
    }

    element_type * get() const BOOST_SP_NOEXCEPT
    {
        return px;
    }

// implicit conversion to "bool"
#include <boost/smart_ptr/detail/operator_bool.hpp>

    bool unique() const BOOST_SP_NOEXCEPT
    {
        return pn.unique();
    }

    long use_count() const BOOST_SP_NOEXCEPT
    {
        return pn.use_count();
    }

    void swap( shared_ptr & other ) BOOST_SP_NOEXCEPT
    {
        std::swap(px, other.px);
        pn.swap(other.pn);
    }

    template<class Y> bool owner_before( shared_ptr<Y> const & rhs ) const BOOST_SP_NOEXCEPT
    {
        return pn < rhs.pn;
    }

    template<class Y> bool owner_before( weak_ptr<Y> const & rhs ) const BOOST_SP_NOEXCEPT
    {
        return pn < rhs.pn;
    }

    void * _internal_get_deleter( boost::detail::sp_typeinfo_ const & ti ) const BOOST_SP_NOEXCEPT
    {
        return pn.get_deleter( ti );
    }

    void * _internal_get_local_deleter( boost::detail::sp_typeinfo_ const & ti ) const BOOST_SP_NOEXCEPT
    {
        return pn.get_local_deleter( ti );
    }

    void * _internal_get_untyped_deleter() const BOOST_SP_NOEXCEPT
    {
        return pn.get_untyped_deleter();
    }

    bool _internal_equiv( shared_ptr const & r ) const BOOST_SP_NOEXCEPT
    {
        return px == r.px && pn == r.pn;
    }

    boost::detail::shared_count _internal_count() const BOOST_SP_NOEXCEPT
    {
        return pn;
    }

// Tasteless as this may seem, making all members public allows member templates
// to work in the absence of member template friends. (Matthew Langston)

#ifndef BOOST_NO_MEMBER_TEMPLATE_FRIENDS

private:

    template<class Y> friend class shared_ptr;
    template<class Y> friend class weak_ptr;


#endif

    element_type * px;                 // contained pointer
    boost::detail::shared_count pn;    // reference counter

};  // shared_ptr
```
　　回归到代理模式，shared_ptr包装了动态分配的对象，通过运算符重载代理了原始指针的行为。  
　　下面代码中通过重载\*，->使得shared_ptr使用和原始指针一样。  
```c++
    typename boost::detail::sp_dereference< T >::type operator* () const BOOST_SP_NOEXCEPT_WITH_ASSERT
    {
        BOOST_ASSERT( px != 0 );
        return *px;
    }
    
    typename boost::detail::sp_member_access< T >::type operator-> () const BOOST_SP_NOEXCEPT_WITH_ASSERT
    {
        BOOST_ASSERT( px != 0 );
        return px;
    }
```