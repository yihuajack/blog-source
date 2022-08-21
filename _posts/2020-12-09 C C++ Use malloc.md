---
title: C/C++使用malloc为结构体数组分配内存（及free释放内存）的三种方法
categories: Programming Language
tags:
  - c
  - c++
  - malloc
  - free
  - struct
date: 2020-12-09 02:05:58
---

以

```c++
template<typename Key, typename Value>
struct Node {};
```


为例，试创建有n个Node类型的node的数组。

**方法一**（nodes[i]为指针）：

```c++
struct Node<int, int> *nodes[n];
for (size_t i = 0; i < n; i++)
    nodes[i] = (struct Node<int, int>*)malloc(sizeof(struct Node<int, int>) * n); 
```

使用

```c++
for (size_t i = 0; i < n; i++) {
    std::free(nodes[i]);
}
```


释放nodes的内存。

**方法二**（nodes[i]为指针）：

```c++
struct Node<int, int> **nodes = (struct Node<int, int>**)malloc(sizeof(struct Node<int, int>*) * n);
for (size_t i = 0; i < n; i++)
    nodes[i] = (struct Node<int, int>*)malloc(sizeof(struct Node<int, int>));
```

使用

```c++
for (size_t i = 0; i < n; i++) {
    std::free(nodes[i]);
}
std::free(nodes);
```

释放nodes的内存。

**方法三**（nodes[i]为结构体struct）：

```c++
struct Node<int, int> *nodes = (struct Node<int, int>*)std::malloc(sizeof(struct Node<int, int>) * n);
```

使用

```c++
std::free(nodes);
```

释放nodes的内存。如果将nodes插入到类中，注意应考虑类的析构函数。

错误方法：

```c++
struct Node<int, int> (*nodes)[n] = (struct Node<int, int>(*)[n])std::malloc(sizeof(struct Node<int, int>) * n);
```


（参考[How to dynamically allocate a 2D array in C?](https://www.geeksforgeeks.org/dynamically-allocate-2d-array-c/)及[malloc申请二维数组的四种方法](https://blog.csdn.net/weixin_43480094/article/details/83932634)）