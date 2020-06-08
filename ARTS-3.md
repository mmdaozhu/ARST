---
layout: post
title: ARST(三)
date: 2020-05-04 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=red>Algorithm</font>

#### **【LeetCode:41. First Missing Positive】**

##### 　　Description:
　　Given an unsorted integer array, find the smallest missing positive integer.

##### 　　Example 1:
```sh
Input: [1,2,0]
Output: 3
```

##### 　　Example 2:
```sh
Input: [3,4,-1,1]
Output: 2
```

##### 　　Example 3:
```sh
Input: [7,8,9,11,12]
Output: 1
```

##### 　　Example 4:
```sh
Input: [2,1]
Output: 3
```

##### 　　Note: 
　　Your algorithm should run in O(n) time and uses constant extra space.
	
##### 　　C++ Solution (failed):
```cpp
/*
解体思路：
    将这组数据以自身为index塞到申请的数组中，并留下标志。
    迭代数组，找到第一个没有标志的元素，并返回index。	

失败原因：
    申请内存太大
*/

class Solution {
public:
    int firstMissingPositive(std::vector<int>& nums) {
        int n{2147483647};
        std::vector<int> array(n);
        for (auto it : nums) {
            if (it > 0) {
                array[it - 1] = 1;
            }
        }

        for (unsigned int i = 0; i < array.size(); i++) {
            if (array[i] == 0) {
                return i + 1;
            }
        }
        return -1;
    }
};
```

##### 　　C++ Solution :
```cpp
/*
解体思路：
    申请一个size比一组数据个数大1的数组array。
    迭代这组数据，忽略非正整数。如果数据元素值小于数组array的size，以自身为index塞到数组array中，并留下标志。
    最好迭代数组array，找到第一个没有标志的元素，并返回index。	

时间复杂度分析：O(n)
*/

#include <assert.h>

#include <iostream>
#include <vector>

class Solution {
public:
    int firstMissingPositive(std::vector<int>& nums) {
        int size = nums.size();
        std::vector<int> array(size + 1);
        for (unsigned int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0 && nums[i] < array.size()) {
                array[nums[i]] = 1;
            }
        }
        for (unsigned int i = 1; i < array.size(); i++) {
            if (array[i] == 0) {
                return i;
            }
        }
        return array.size();
    }
};

```

###  <font color=red>Review</font>

#### **【Article】**
http://www.aosabook.org/en/distsys.html (1.2. The Basics[Redundancy, Partitions])

#### **【Words】**

English | Chinese
-|-
degrade | vt. 贬低；使……丢脸；使……降级；使……降解
failover | n. （计算机）故障切换
intervention | n. 介入；调停；妨碍
diminish | vt. 使减少；使变小
intrinsic  | adj. 本质的，固有的
cumbersome  | adj. 笨重的；累赘的；难处理的
obstacle | n. 障碍，干扰，妨碍；障碍物
inconsistency | n. 不一致；易变

#### **【Comments and Conclusions】**
　　Creating redundancy in a system can remove single points of failure and provide a backup or spare functionality if needed in a crisis. Another key part of service redundancy is creating a shared-nothing architecture.  
　　When there may be a very large data sets that are unable to fit on a single server. To scale horizontally is to add more nodes.  
　　Of course there are challenges, One of the key issues is data locality, another potential issues comes in the form of inconsistency.  

###  <font color=red>tips：Docker 常用命令</font>

#### **【镜像查看】**
```sh
docker images
```

#### **【镜像下载】**
```sh
docker pull redis
```

#### **【镜像删除】**
```sh
docker rmi redis

docker rmi -f redis
```

#### **【生成镜像】**
```sh
docker commit afcaf46e8305 myredis
```

#### **【容器启动】**
```sh
#-i: 以交互模式运行容器，通常与 -t 同时使用；
#-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
#--volume , -v: 绑定一个卷

docker run -ti ubuntu bash
docker run -ti -v /AAA:/BBB ubuntu bash
```

#### **【在运行的容器中执行命令】**
```sh
#-i :即使没有附加也保持STDIN 打开
#-t :分配一个伪终端
docker exec -ti  myredis /bin/bash
```

#### **【查看容器】**
```sh
docker ps
```

###  <font color=red>Share: 剖析C++ std::sort函数源码</font>

#### **【快排的原理】**
　　首先我们先了解下排序算法中最经典的算法：快速排序。几乎所有的编程语言都会提供排序函数，大多数都是基于快速排序算法实现的，比如我们要讲的std::sort函数。  
　　快排的思想是这样的：如果要排序数组中下标从p到r之间的一组数据，我们选择p到r之间的任意一个数据作为pivot（分区点）。  
　　我们遍历p到r之间的数据，将小于pivot的放到左边，将大于pivot的放到右边，将pivot放到中间。经过这一个步骤之后，数组p到r之间的数据被分成了三部分，前面p到q-1之间的都小于pivot，中间是pivot，后面的q+1到r之间大于pivot。  
　　根据分治递归的处理思想，我们可以递归排序下标p到q-1之间的数据和下标q+1到r之间的数据，直到区间缩小为1，就说明所有的数据都有序了。  
　　快排的时间复杂度是O(nlogn)的，是原地排序和不稳定排序。  

#### **【快排的实现】**
　　我们现在用c++来实现一遍，取一组数据中的最后一个数据作为分区点。  
```c++
void quicksort(int p, int r, std::vector<int>& nums) {
    if (p >= r) {
        return;
    }
    int i = p;
    for (int j = p; j < r; j++) {
        if (nums[j] < nums[r]) {
            std::swap(nums[i], nums[j]);
            i++;
        }
    }
    std::swap(nums[i], nums[r]);
    quicksort(p, i - 1, nums);
    quicksort(i + 1, r, nums);
}
```
　　快排的实现最重要的是i指针，每一轮迭代后，i指针指向的元素始终大于(等于)分区点的元素。迭代结束后，交换i指针指向的元素和分区点的元素。记住这一点，快排就很好实现了。  

#### **【std::sort函数源码剖析】**
　　首先我们来看下sort函数的源码，里面调用了std::\_\_sort函数，传递了\_\_gnu_cxx::\_\_ops::\_\_iter_less_iter()函数对象，使得数列从小到大排序。  
```c++
// /usr/include/c++/7.4.0/bits/stl_algo.h

 /**
   *  @brief Sort the elements of a sequence.
   *  @ingroup sorting_algorithms
   *  @param  __first   An iterator.
   *  @param  __last    Another iterator.
   *  @return  Nothing.
   *
   *  Sorts the elements in the range @p [__first,__last) in ascending order,
   *  such that for each iterator @e i in the range @p [__first,__last-1),  
   *  *(i+1)<*i is false.
   *
   *  The relative ordering of equivalent elements is not preserved, use
   *  @p stable_sort() if this is needed.
  */
  template<typename _RandomAccessIterator>
    inline void
    sort(_RandomAccessIterator __first, _RandomAccessIterator __last)
    {
      // concept requirements
      __glibcxx_function_requires(_Mutable_RandomAccessIteratorConcept<
	    _RandomAccessIterator>)
      __glibcxx_function_requires(_LessThanComparableConcept<
	    typename iterator_traits<_RandomAccessIterator>::value_type>)
      __glibcxx_requires_valid_range(__first, __last);
      __glibcxx_requires_irreflexive(__first, __last);

      std::__sort(__first, __last, __gnu_cxx::__ops::__iter_less_iter());
    }
```

　　__sort函数调用了内省排序和插入排序。内省排序首先从快速排序开始，当递归深度超过一定深度后转为堆排序。std::\_\_lg函数是用来计算递归深度。  
```c++
// /usr/include/c++/7.4.0/bits/stl_algo.h

  // sort
  template<typename _RandomAccessIterator, typename _Compare>
    inline void
    __sort(_RandomAccessIterator __first, _RandomAccessIterator __last,
	   _Compare __comp)
    {
      if (__first != __last)
	{
	  std::__introsort_loop(__first, __last,
				std::__lg(__last - __first) * 2,
				__comp);
	  std::__final_insertion_sort(__first, __last, __comp);
	}
    }
```

　　我们来分析一下内省排序\_\_introsort_loop函数。  
　　while循环里面的判断条件是排序区间大小。如果太小了，就直接退化成插入排序了。在小规模数据面前，O(n<sup>2</sup>)的时间复杂度的算法并不一定比O(nlogn)的算法执行时间长。  
　　当递归深度超过一定的深度，则使用std::__partial_sort(堆排序)。堆排序就不详细展开了。  
　　\_\_unguarded_partition_pivot函数是选择三数取中作为分区点，并将中位数放在数列的开头。  
　　接着递归调用\_\_introsort_loop函数。  
　　最后我们来重点看下第21行代码，这边的实现和我上面实现的排序算法有点不一样。它是通过循环来处理右边子序列的，这样的一个好处就是减少函数的调用。令人惊叹！  
```c++
// /usr/include/c++/7.4.0/bits/stl_algo.h

  /// This is a helper function for the sort routine.
  template<typename _RandomAccessIterator, typename _Size, typename _Compare>
    void
    __introsort_loop(_RandomAccessIterator __first,
		     _RandomAccessIterator __last,
		     _Size __depth_limit, _Compare __comp)
    {
      while (__last - __first > int(_S_threshold))
	{
	  if (__depth_limit == 0)
	    {
	      std::__partial_sort(__first, __last, __last, __comp);
	      return;
	    }
	  --__depth_limit;
	  _RandomAccessIterator __cut =
	    std::__unguarded_partition_pivot(__first, __last, __comp);
	  std::__introsort_loop(__cut, __last, __depth_limit, __comp);
	  __last = __cut;
	}
    }
	
....
....
....
	  /// This is a helper function...
    template<typename _RandomAccessIterator, typename _Compare>
    inline _RandomAccessIterator
    __unguarded_partition_pivot(_RandomAccessIterator __first,
				_RandomAccessIterator __last, _Compare __comp)
    {
      _RandomAccessIterator __mid = __first + (__last - __first) / 2;
      std::__move_median_to_first(__first, __first + 1, __mid, __last - 1,
				  __comp);
      return std::__unguarded_partition(__first + 1, __last, __first, __comp);
    }
```

　　接着我们来看看__unguarded_partition函数，也就是快速排序的主体部分。  
　　它使用first和last两个指针不断交换放错位置的元素，直到first和last相互交错为止，最后返回右边区间的起始位置。  
```c++
// /usr/include/c++/7.4.0/bits/stl_algo.h

  /// This is a helper function...
  template<typename _RandomAccessIterator, typename _Compare>
    _RandomAccessIterator
    __unguarded_partition(_RandomAccessIterator __first,
			  _RandomAccessIterator __last,
			  _RandomAccessIterator __pivot, _Compare __comp)
    {
      while (true)
	{
	  while (__comp(__first, __pivot))
	    ++__first;
	  --__last;
	  while (__comp(__pivot, __last))
	    --__last;
	  if (!(__first < __last))
	    return __first;
	  std::iter_swap(__first, __last);
	  ++__first;
	}
    }
```
　　std::\_\_sort函数中调完std::\_\_introsort_loop函数之后，我们的数列是相对有序的，最后再使用插入排序。  
