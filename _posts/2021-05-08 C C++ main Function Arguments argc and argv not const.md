---
title: C语言/C++ main函数参数argc和argv不是const的原因
categories: Software
tags:
  - c语言
  - c++
date: 2021-05-08 20:07:16
---

 翻译自：[Why is argc not a constant?](https://stackoverflow.com/questions/20558418/why-is-argc-not-a-constant)

```c++
int main(int argc, char *argv[])
```

如[Effective C++](https://rads.stackoverflow.com/amzn/click/0321334876) Item#3指出"应尽可能使用const"，为何不将argc和argv默认设为const？在何种情景下argc会被修改？

对此问题的回答是：

该问题有历史因素。C将这些参数定义为"非常量"以兼容现有的C代码。诸如getopt等的UNIX API会修改argv[]，所以不能被设为const。有趣的是，getopt的原型表明它不会修改argv[]本身，而可能修改其指向的地址：[it appears they know they're being naughty](http://man7.org/linux/man-pages/man3/getopt.3.html#CONFORMING_TO).。

给argc和argv加上const不会获得什么益处，而且这会使得一些老旧的编程练习失效：

```c++
// print out all the arguments:
while (--argc)
    std::cout << *++argv << std::endl;
```
