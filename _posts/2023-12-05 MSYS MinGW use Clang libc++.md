---
title: MSYS MinGW使用Clang libc++的方法
categories: Programming Language
tags:
  - c++
  - 开发语言
  - clang
  - libc++
  - mingw
date: 2023-12-05 10:56:44
---

今天在使用 Clang 测试 Formatting Ranges（P2286R8）的时候报错 static assertion failed，查询报错信息发现 Clang 编译器使用的库文件为 msys64/mingw64/include/c++/13.2.0/format 和 msys64/mingw64/include/c++/13.2.0/concepts，而这是 GCC libstdc++ 13.2.0 的库文件。若要让 Clang 使用 libc++（即到目前为止唯一完整支持 P2286R8 的 STL 库）则需要执行以下步骤：
1. 安装 Clang 和 libc++：
```bash
pacman -Syu
pacman -S mingw-w64-x86_64-clang
pacman -S mingw-w64-x86_64-libc++
```
2. 在编译时加上`-stdlib=libc++`选项：
```bash
clang++ -std=c++23 -stdlib=libc++ test.cpp
```