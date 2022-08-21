---
title: 为什么std::optional的构造函数使用std::in_place
categories: Programming Language
tags:
  - c++
  - in_place
  - optional
  - c++17
  - emplace
date: 2021-06-10 15:06:31
---

参考https://stackoverflow.com/questions/49767940/why-do-stdoptional-constructors-use-stdin-place/49768728。

是用来区分到底是用optional<T>的默认构造函数还是T的默认构造函数，参见[N3527](https://link.zhihu.com/?target=http%3A//www.open-std.org/jtc1/sc22/wg21/docs/papers/2013/n3527.html%23rationale.emplace)，in_place_t原本的名字是emplace：

> 该提议提供了一个'in_place'构造函数能将optional的构造函数的参数完美转发给T的构造函数。为了触发这个构造函数我们应该使用标签结构emplace。我们需要额外的标签来区分一些情况，比如是调用optional的默认构造函数还是要T的默认构造函数：
>
> ```c++
> optional<Big> ob{emplace, "1"}; // 调用构造函数 Big{"1"} （不移动）
> optional<Big> oc{emplace};      // 调用构造函数 Big{} （不移动）
> optional<Big> od{};             // 创建一个 disengaged 的 optional
> ```
>
> ……另一方面，在很多容器类型中emplace在std:中被重载成既是成员函数也是标签，如果这个问题很严重，那么标签（emplace作为标签的情况）可以被重命名为in_place。

本文同步在[c++17 std::in_place 这几个东西是干什么的？？](https://www.zhihu.com/question/66032208/answer/1932909711) 
