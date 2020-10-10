---
layout: post
title: ARST(七)
date: 2020-06-01 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=orange>Algorithm</font>

#### **【LeetCode:150. Evaluate Reverse Polish Notation】**

##### 　　Description:
　　Evaluate the value of an arithmetic expression in Reverse Polish Notation.  
　　Valid operators are +, -, \*, /. Each operand may be an integer or another expression.  

##### 　　Note:
　　- Division between two integers should truncate toward zero.  
　　- The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.  

##### 　　Example 1:
```sh
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

##### 　　Example 2:
```sh
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

##### 　　Example 3:
```sh
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

##### 　　C++ Solution:
https://github.com/mmdaozhu/leetcode/blob/master/cpp/150.EvaluateReversePolishNotation/EvaluateReversePolishNotation.cpp

###  <font color=orange>Review</font>

#### **【Article】**
https://github.com/donnemartin/system-design-primer (https://www.youtube.com/watch?v=-W9F__D3oY4)

#### **【Words】**

English | Chinese
-|-
scattered  | adj. 分散的；散乱的
flashcard  | n. 教学用的抽认卡
deck | n. 甲板
repetition | n. 重复；背诵；副本
polish | v. 抛光，擦亮；修改，润色；（使）完美，改进
pro | n. 赞成者；赞成的意见
con | n. 诈骗，骗局；反对票
outline | n. 轮廓；大纲；概要；略图
constraint | n. [数] 约束；限制；约束条件
envelope | n. 信封，封皮
ace | vt. 以发球赢一分；击败


#### **【Comments and Conclusions】**

##### 1. Vertical Scaling
　　add more resources(CPU/RAM/DISK) to your server as on demand.  
　　- CPU: cores, L2 Cache  
　　- DISK: PATA, SATA, SAS, ... RAID(HDD use SATA and SAS interface, SSD use SATA, MSATA and M.2 interface)  
　　- RAM  

##### 2. Load Balancing
　　DNS  

##### 3. Shared Session State
　　Using database  　

##### 4. RAID
　　- RAID 0: fastest, no redundance.  
　　- RAID 1: consist of data mirroring, write throughput is slower.  
　　- RAID 5: require at least three disks, a tradeoff between RAID 0 and RAID 1.  
　　- RAID 6: fault tolerance up to two failed drives.  
　　- RAID 10: consist of RAID 0 and RAID 1.  

##### 5. Shared Storage
　　- FC: Fibre Channel  
　　- iSCSI: internet small computer system interface.(protocol)  
　　- MySQL  
　　- NFS: network File System  
　
###  <font color=orange>tips：Maven POM如何引入外部依赖</font>
　　POM( Project Object Model，项目对象模型 ) 是 Maven 工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构建，声明项目依赖，等等。执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。  

#### **【最简单的POM文件解析】**
```xml
<project xmlns = "http://maven.apache.org/POM/4.0.0"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
 
    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    <!-- 公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.companyname.project-group，maven会将该项目打成的jar包放本地路径：/com/companyname/project-group -->
    <groupId>com.companyname.project-group</groupId>
 
    <!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
    <artifactId>project</artifactId>
 
    <!-- 版本号 -->
    <version>1.0</version>
</project>
```

#### **【Maven 引入外部依赖】**
　　pom.xml 的 dependencies 列表列出了我们的项目需要构建的所有外部依赖项。  
　　要添加依赖项，我们一般是先在 src 文件夹下添加 lib 文件夹，然后将你工程需要的 jar 文件复制到 lib 文件夹下。我们使用的是 ldapjdk.jar。  
　　然后添加以下依赖到 pom.xml 文件中：  
```xml
<dependencies>
    <!-- 在这里添加你的依赖 -->
    <dependency>
        <groupId>ldapjdk</groupId>  <!-- 库名称，也可以自定义 -->
        <artifactId>ldapjdk</artifactId>    <!--库名称，也可以自定义-->
        <version>1.0</version> <!--版本号-->
        <scope>system</scope> <!--作用域-->
        <systemPath>${basedir}\src\lib\ldapjdk.jar</systemPath> <!--项目根目录下的lib文件夹下-->
    </dependency> 
</dependencies>
```
　　如果不需要某个依赖，则使用exclusions标签。  
```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
```

###  <font color=orange>Share：剖析C++ std::priority_queue源码</font>

#### **【优先级队列】**
　　优先级队列，首先是一个队列，队列的最大特性就是先进先出。不过在优先级队列中，数据的出队顺序不是先进先出，按照优先级出列，优先级高的，最先出队。  
　　C++ 的 priority_queue 是使用堆来实现优先级队列的。  

#### **【priority_queue的定义】**
　　class priority_queue的定义如下：  
```c++
// /usr/include/c++/7.4.0/bits/stl_queue.h
  /**
   *  @brief  A standard container automatically sorting its contents.
   *
   *  @ingroup sequences
   *
   *  @tparam _Tp  Type of element.
   *  @tparam _Sequence  Type of underlying sequence, defaults to vector<_Tp>.
   *  @tparam _Compare  Comparison function object type, defaults to
   *                    less<_Sequence::value_type>
  */

  template<typename _Tp, typename _Sequence = vector<_Tp>,
	   typename _Compare  = less<typename _Sequence::value_type> >
    class priority_queue
```
　　第一个template参数是元素的类型，带有默认值的第二个template参数是定义了priority_queue的内部用来存放元素的容器，默认容器是vector。带有默认值的第三个参数template是定义了“用以查找下一个最高优先级元素”的排序规则，默认是以operator < 作为比较标准。  
　　_Sequence容器必须支持random-access iterator 和 front(), push_back(), pop_back()等操作。  

#### **【priority_queue的默认构造函数】**
　　priority_queue的默认构造函数定义如下：  
```c++
// /usr/include/c++/7.4.0/bits/stl_queue.h
    protected:
      //  See queue::c for notes on these names.
      _Sequence  c;
      _Compare   comp;

    public:
      /**
       *  @brief  Default constructor creates no elements.
       */
#if __cplusplus < 201103L
      explicit
      priority_queue(const _Compare& __x = _Compare(),
		     const _Sequence& __s = _Sequence())
      : c(__s), comp(__x)
      { std::make_heap(c.begin(), c.end(), comp); }
#else
      template<typename _Seq = _Sequence, typename _Requires = typename
	       enable_if<__and_<is_default_constructible<_Compare>,
				is_default_constructible<_Seq>>::value>::type>
	priority_queue()
	: c(), comp() { }

      explicit
      priority_queue(const _Compare& __x, const _Sequence& __s)
      : c(__s), comp(__x)
      { std::make_heap(c.begin(), c.end(), comp); }

      explicit
      priority_queue(const _Compare& __x, _Sequence&& __s = _Sequence())
      : c(std::move(__s)), comp(__x)
      { std::make_heap(c.begin(), c.end(), comp); }

#endif
```
　　我们注意到构造函数中其实调用了std::make_heap函数来建堆，之后我们着重讲解这个函数。  

#### **【priority_queue的核心接口】**
　　priority_queue的核心接口定义如下：  
```c++
// /usr/include/c++/7.4.0/bits/stl_queue.h

      /**
       *  Returns true if the %queue is empty.
       */
      bool
      empty() const
      { return c.empty(); }

      /**  Returns the number of elements in the %queue.  */
      size_type
      size() const
      { return c.size(); }

      /**
       *  Returns a read-only (constant) reference to the data at the first
       *  element of the %queue.
       */
      const_reference
      top() const
      {
	__glibcxx_requires_nonempty();
	return c.front();
      }

      /**
       *  @brief  Add data to the %queue.
       *  @param  __x  Data to be added.
       *
       *  This is a typical %queue operation.
       *  The time complexity of the operation depends on the underlying
       *  sequence.
       */
      void
      push(const value_type& __x)
      {
	c.push_back(__x);
	std::push_heap(c.begin(), c.end(), comp);
      }

#if __cplusplus >= 201103L
      void
      push(value_type&& __x)
      {
	c.push_back(std::move(__x));
	std::push_heap(c.begin(), c.end(), comp);
      }

      template<typename... _Args>
	void
	emplace(_Args&&... __args)
	{
	  c.emplace_back(std::forward<_Args>(__args)...);
	  std::push_heap(c.begin(), c.end(), comp);
	}
#endif

      /**
       *  @brief  Removes first element.
       *
       *  This is a typical %queue operation.  It shrinks the %queue
       *  by one.  The time complexity of the operation depends on the
       *  underlying sequence.
       *
       *  Note that no data is returned, and if the first element's
       *  data is needed, it should be retrieved before pop() is
       *  called.
       */
      void
      pop()
      {
	__glibcxx_requires_nonempty();
	std::pop_heap(c.begin(), c.end(), comp);
	c.pop_back();
      }
```
　　我们重点看下push()，pop()两个函数。其实是分别调用了std::push_heap()，std::pop_heap()两个函数。其它的接口还是挺简单的。  
　　下面我们来重点分析下建堆、往堆中插入一个元素和删除堆顶元素这几个过程。  

#### **【堆操作函数】**
　　接下来是最难难理解的堆操作函数源码部分。  
　　为了方便理解源码，我们要知道如果一个节点的下标是i，那么左子节点的下标就是2\*i+1，右子节点的下标就是2\*i+2，父节点的下标就是(i-1)/2。  
　　我们把__comp当作operator<规则，也就是这个堆是个大顶堆。  

##### 1. 往堆中插入一个元素
　　因为建堆的过程涉及到__push_heap函数操作，所以我们第一个先剖析往堆中插入一个元素操作。  
　　源码如下：  
```c++
// /usr/include/c++/7.4.0/bits/stl_queue.h

  template<typename _RandomAccessIterator, typename _Distance, typename _Tp,
	   typename _Compare>
    void
    __push_heap(_RandomAccessIterator __first,
		_Distance __holeIndex, _Distance __topIndex, _Tp __value,
		_Compare& __comp)
    {
      _Distance __parent = (__holeIndex - 1) / 2;
      while (__holeIndex > __topIndex && __comp(__first + __parent, __value))
	{
	  *(__first + __holeIndex) = _GLIBCXX_MOVE(*(__first + __parent));
	  __holeIndex = __parent;
	  __parent = (__holeIndex - 1) / 2;
	}
      *(__first + __holeIndex) = _GLIBCXX_MOVE(__value);
    }

  /**
   *  @brief  Push an element onto a heap using comparison functor.
   *  @param  __first  Start of heap.
   *  @param  __last   End of heap + element.
   *  @param  __comp   Comparison functor.
   *  @ingroup heap_algorithms
   *
   *  This operation pushes the element at __last-1 onto the valid
   *  heap over the range [__first,__last-1).  After completion,
   *  [__first,__last) is a valid heap.  Compare operations are
   *  performed using comp.
  */
  template<typename _RandomAccessIterator, typename _Compare>
    inline void
    push_heap(_RandomAccessIterator __first, _RandomAccessIterator __last,
	      _Compare __comp)
    {
      typedef typename iterator_traits<_RandomAccessIterator>::value_type
	  _ValueType;
      typedef typename iterator_traits<_RandomAccessIterator>::difference_type
	  _DistanceType;

      // concept requirements
      __glibcxx_function_requires(_Mutable_RandomAccessIteratorConcept<
	    _RandomAccessIterator>)
      __glibcxx_requires_valid_range(__first, __last);
      __glibcxx_requires_irreflexive_pred(__first, __last, __comp);
      __glibcxx_requires_heap_pred(__first, __last - 1, __comp);

      __decltype(__gnu_cxx::__ops::__iter_comp_val(_GLIBCXX_MOVE(__comp)))
	__cmp(_GLIBCXX_MOVE(__comp));
      _ValueType __value = _GLIBCXX_MOVE(*(__last - 1));
      std::__push_heap(__first, _DistanceType((__last - __first) - 1),
		       _DistanceType(0), _GLIBCXX_MOVE(__value), __cmp);
    }
```
　　从源码中可以看出，\[\_\_first,\_\_last-1)是一个有效堆，通过push_heap函数使得\[\_\_first,\_\_last)变成一个有效堆。  
　　这个\_\_holeIndex可以理解为我现在要插入一个元素，但是我并不知道要插在哪里，所以我先挖一个坑，然后经过一系列的算法，这个\_\_holeIndex，要开始慢慢往上爬，以满足大顶堆条件。  
　　步骤：  
　　- 1. 保存\_last-1元素的value值。  
　　- 2. \_\_holeIndex往parent节点爬。  
　　- 3. 如果parent节点比value值小，父节点的值会填充\_\_holeIndex节点，然后继续执行步骤2。  
　　- 4. 如果parent节点比value值大，此时\_\_holeIndex已经上溯到正确的位置，只需要将value值填到\_\_holeIndex节点即可。  
　　我这里画了一张堆化的过程分解图，其实这就是从下往上的堆化方法。（后面贴上）  

##### 2. 删除堆顶元素
　　源码如下：  
```c++
// /usr/include/c++/7.4.0/bits/stl_queue.h

  template<typename _RandomAccessIterator, typename _Distance,
	   typename _Tp, typename _Compare>
    void
    __adjust_heap(_RandomAccessIterator __first, _Distance __holeIndex,
		  _Distance __len, _Tp __value, _Compare __comp)
    {
      const _Distance __topIndex = __holeIndex;
      _Distance __secondChild = __holeIndex;
      while (__secondChild < (__len - 1) / 2)
	{
	  __secondChild = 2 * (__secondChild + 1);
	  if (__comp(__first + __secondChild,
		     __first + (__secondChild - 1)))
	    __secondChild--;
	  *(__first + __holeIndex) = _GLIBCXX_MOVE(*(__first + __secondChild));
	  __holeIndex = __secondChild;
	}
      if ((__len & 1) == 0 && __secondChild == (__len - 2) / 2)
	{
	  __secondChild = 2 * (__secondChild + 1);
	  *(__first + __holeIndex) = _GLIBCXX_MOVE(*(__first
						     + (__secondChild - 1)));
	  __holeIndex = __secondChild - 1;
	}
      __decltype(__gnu_cxx::__ops::__iter_comp_val(_GLIBCXX_MOVE(__comp)))
	__cmp(_GLIBCXX_MOVE(__comp));
      std::__push_heap(__first, __holeIndex, __topIndex,
		       _GLIBCXX_MOVE(__value), __cmp);
    }

  template<typename _RandomAccessIterator, typename _Compare>
    inline void
    __pop_heap(_RandomAccessIterator __first, _RandomAccessIterator __last,
	       _RandomAccessIterator __result, _Compare& __comp)
    {
      typedef typename iterator_traits<_RandomAccessIterator>::value_type
	_ValueType;
      typedef typename iterator_traits<_RandomAccessIterator>::difference_type
	_DistanceType;

      _ValueType __value = _GLIBCXX_MOVE(*__result);
      *__result = _GLIBCXX_MOVE(*__first);
      std::__adjust_heap(__first, _DistanceType(0),
			 _DistanceType(__last - __first),
			 _GLIBCXX_MOVE(__value), __comp);
    }

   /**
   *  @brief  Pop an element off a heap using comparison functor.
   *  @param  __first  Start of heap.
   *  @param  __last   End of heap.
   *  @param  __comp   Comparison functor to use.
   *  @ingroup heap_algorithms
   *
   *  This operation pops the top of the heap.  The elements __first
   *  and __last-1 are swapped and [__first,__last-1) is made into a
   *  heap.  Comparisons are made using comp.
  */
  template<typename _RandomAccessIterator, typename _Compare>
    inline void
    pop_heap(_RandomAccessIterator __first,
	     _RandomAccessIterator __last, _Compare __comp)
    {
      // concept requirements
      __glibcxx_function_requires(_Mutable_RandomAccessIteratorConcept<
	    _RandomAccessIterator>)
      __glibcxx_requires_valid_range(__first, __last);
      __glibcxx_requires_irreflexive_pred(__first, __last, __comp);
      __glibcxx_requires_non_empty_range(__first, __last);
      __glibcxx_requires_heap_pred(__first, __last, __comp);

      if (__last - __first > 1)
	{
	  typedef __decltype(__comp) _Cmp;
	  __gnu_cxx::__ops::_Iter_comp_iter<_Cmp> __cmp(_GLIBCXX_MOVE(__comp));
	  --__last;
	  std::__pop_heap(__first, __last, __last, __cmp);
	}
    }
```
　　首先我们先来看下\_\_pop_heap函数，它先把堆中的\_\_last-1保存下来，然后将\_\_first放在\_\_last-1位置上，其实就是将堆顶元素放到容器最后。  
　　此时堆顶是空的，\_\_holeIndex指向这个空洞，这个\_\_holeIndex，与\_\_push_heap函数中的相反，慢慢往下爬，以满足大顶堆条件。  
　　步骤：  
　　- 1. 以\_\_holeIndex节点，比较它的左右孩子节点的值。值大的节点的值会填充\_\_holeindex节点。（如果没有右子树，就把右子树移上去）  
　　- 2. \_\_holeIndex往下爬，指向步骤1中值大的节点。重复步骤1，直到\_\_holeIndex爬到最底层。  
　　- 3. 针对\_\_holeIndex节点再执行一次\_\_push_heap操作，使其满足大顶堆。  
　　我这里画了一张堆化的过程分解图，其实这就是从上往下的堆化方法。（后面贴上）  

##### 3. 建堆
　　先看源码：  
```c++
// /usr/include/c++/7.4.0/bits/stl_heap.h

  template<typename _RandomAccessIterator, typename _Compare>
    void
    __make_heap(_RandomAccessIterator __first, _RandomAccessIterator __last,
		_Compare& __comp)
    {
      typedef typename iterator_traits<_RandomAccessIterator>::value_type
	  _ValueType;
      typedef typename iterator_traits<_RandomAccessIterator>::difference_type
	  _DistanceType;

      if (__last - __first < 2)
	return;

      const _DistanceType __len = __last - __first;
      _DistanceType __parent = (__len - 2) / 2;
      while (true)
	{
	  _ValueType __value = _GLIBCXX_MOVE(*(__first + __parent));
	  std::__adjust_heap(__first, __parent, __len, _GLIBCXX_MOVE(__value),
			     __comp);
	  if (__parent == 0)
	    return;
	  __parent--;
	}
    }

  /**
   *  @brief  Construct a heap over a range using comparison functor.
   *  @param  __first  Start of heap.
   *  @param  __last   End of heap.
   *  @param  __comp   Comparison functor to use.
   *  @ingroup heap_algorithms
   *
   *  This operation makes the elements in [__first,__last) into a heap.
   *  Comparisons are made using __comp.
  */
  template<typename _RandomAccessIterator, typename _Compare>
    inline void
    make_heap(_RandomAccessIterator __first, _RandomAccessIterator __last,
	      _Compare __comp)
    {
      // concept requirements
      __glibcxx_function_requires(_Mutable_RandomAccessIteratorConcept<
	    _RandomAccessIterator>)
      __glibcxx_requires_valid_range(__first, __last);
      __glibcxx_requires_irreflexive_pred(__first, __last, __comp);

      typedef __decltype(__comp) _Cmp;
      __gnu_cxx::__ops::_Iter_comp_iter<_Cmp> __cmp(_GLIBCXX_MOVE(__comp));
      std::__make_heap(__first, __last, __cmp);
    }
```
　　分析完前面的堆操作源码，现在来看建堆操作源码就简单多了。  
　　\_\_make_heap是将一段现有的数据转化成堆。操作就是从最后一个非叶子进行调堆操作开始，直到第一个非叶子节点（根节点）进行调堆操作结束。这时整个堆满足大顶堆。  
　　我这里画了一张堆化的过程分解图，是从上往下的堆化方法。（后面贴上）  