---
layout: post
title: ARST(五)
date: 2020-05-18 9:30:00
tags: 
	- growing
	- ARST
---

###  <font color=orange>Algorithm</font>

#### **【LeetCode:23. Merge k Sorted Lists】**

##### 　　Description:
　　Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.  

##### 　　Example 1:
```sh
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

##### 　　C++ Solution:
```cpp
/*
解体思路：
    由于每个链表都是有序的，我们只要依次比较链表头，取出最小值的链表头。
    然后在最小值链表头所在链表取下一个元素，重复上述过程。

时间复杂度分析：O(k*n)
*/

#include <assert.h>

#include <iostream>
#include <limits>
#include <vector>

struct ListNode {
    int val;
    ListNode* next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode* next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode* mergeKLists(std::vector<ListNode*>& lists) {
        if (lists.size() == 0) {
            return nullptr;
        }
        if (lists.size() == 1) {
            return lists[0];
        }

        ListNode* res = new ListNode(0);
        ListNode* connect = res;
        std::vector<ListNode*> pv = lists;
        while (true) {
            int min = std::numeric_limits<int>::max();
            int index = -1;
            for (size_t i = 0; i < pv.size(); i++) {
                if (pv[i] != nullptr && pv[i]->val < min) {
                    min = pv[i]->val;
                    index = i;
                }
            }

            if (index != -1) {
                connect->next = pv[index];
                connect = connect->next;
                pv[index] = pv[index]->next;
            } else {
                break;
            }
        }
        return res->next;
    }
};
```

###  <font color=orange>Review</font>

#### **【Article】**
http://www.aosabook.org/en/distsys.html (1.3. The Building Blocks of Fast and Scalable Data Access[Distributed Cache, Proxies, Indexes])  

#### **【Words】**

English | Chinese
-|-
remedy | v. 补救，纠正，改进；治疗
catastrophic | adj. 灾难的；悲惨的；灾难性的，毁灭性的



#### **【Comments and Conclusions】**
　　Distributed Cache chart:  
![hello](/ARTS-5/1.png)  
　　1.Distributed cache  

　　Proxies chart:  
![hello](/ARTS-5/2.png)  
　　1.Proxy server  

![hello](/ARTS-5/3.png)  
　　2.Using a proxy server to collapse requests  

![hello](/ARTS-5/4.png)  
　　3.Using a proxy to collapse requests for data that is spatially close together  

　　Index chart:  
![hello](/ARTS-5/5.jpg)  
　　1.Indexes  

![hello](/ARTS-5/6.jpg)  	
　　2.Many layers of indexes  

###  <font color=orange>tips: Linux命令：xargs简介</font>
　　xargs命令是给其他命令传递参数的一个过滤器，也是组合多个命令的一个工具。它擅长将标准输入数据转换成命令行参数，xargs能够处理管道或者stdin并将其转换成特定命令的命令参数。  
```sh
# 将find . 命令结果转换为touch的命令行参数
find . | xargs touch
```

###  <font color=orange>Share：Google Test 的安装和使用</font>

#### **【下载】**
Google Test版本：v1.10.0  
下载链接：https://github.com/google/googletest/releases/tag/release-1.10.0  

#### **【环境】**
docker  
Linux: Ubuntu-18.04  
gcc: 7.4.0  
CMake: 3.18.0-rc1  

#### **【编译】**
```sh
cd googletest-release-1.10.0
cmake .
#result:
-- The C compiler identification is GNU 7.4.0
-- The CXX compiler identification is GNU 7.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Could NOT find PythonInterp (missing: PYTHON_EXECUTABLE)
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Failed
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE
-- Configuring done
-- Generating done
-- Build files have been written to: /root/googletest-release-1.10.0
```

```sh
make
# 编译的库文件在当前lib目录下
```

#### **【使用】**
　　组织一下googletest的头文件和库文件  
```sh
googletest
          - gmock(header files)
          - gtest(header files)
          - libgmock.a
          - libgmock_main.a
          - libgtest.a
          - libgtest_main.a
```
　　编写一个测试代码，示例代码如下：  
```c++
#include <gtest/gtest.h>

#include <iostream>
#include <string>

class Foo {
public:
    Foo() {}
    ~Foo() {}
    int retZero() { return 0; }

    std::string retStringNull() { return ""; }
};

class FooTest : public ::testing::Test {
protected:
    FooTest() {
        // You can do set-up work for each test here.
    }
    ~FooTest() override {
        // You can do clean-up work that does not throw exceptions here.
    }

    // If the constructor and destructor are not enough for setting up
    // and cleaning up each test, you can define the following methods:
    void SetUp() override {
        // Code here will be called immediately after the constructor (right
        // before each test).
    }
    void TearDown() override {
        // Code here will be called immediately after each test (right
        // before the destructor).
    }

    // Class members declared here can be used by all tests in the test suite
    // for Foo.
};

TEST_F(FooTest, testretZero) {
    Foo f;
    EXPECT_EQ(f.retZero(), 0);
}

TEST_F(FooTest, testretStringNull) {
    Foo f;
    EXPECT_EQ(f.retStringNull(), "");
}

int main(int argc, char **argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}

// g++ -std=c++11 -Wall -I googletest/ test.cpp -o test -Lgoogletest/ -lgtest -lgtest_main -lpthread
// result:
// [==========] Running 2 tests from 1 test suite.
// [----------] Global test environment set-up.
// [----------] 2 tests from FooTest
// [ RUN      ] FooTest.testretZero
// [       OK ] FooTest.testretZero (0 ms)
// [ RUN      ] FooTest.testretStringNull
// [       OK ] FooTest.testretStringNull (0 ms)
// [----------] 2 tests from FooTest (0 ms total)

// [----------] Global test environment tear-down
// [==========] 2 tests from 1 test suite ran. (0 ms total)
// [  PASSED  ] 2 tests.
```

