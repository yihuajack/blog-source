---
title: Qt 6 MinGW使用GSL库的方法
categories: DevOps
tags:
  - qt
  - 开发语言
  - cmake
  - gsl
  - qt6
date: 2023-11-02 22:15:41
---

本文参考[Qt5配置开源GSL数学库](https://huangwang.github.io/2019/10/07/Qt5%E9%85%8D%E7%BD%AE%E5%BC%80%E6%BA%90GSL%E6%95%B0%E5%AD%A6%E5%BA%93/)与[Windows系统Qt5/mingw-64配置GSL科学计算库](https://blog.csdn.net/ouening/article/details/82993947)。
首先参考[MSYS2快速配置C++环境+ZSH](https://blog.csdn.net/yihuajack/article/details/108172931)安装MSYS2（64位）并换源（最好安装与Qt Maintenance Tool安装的Qt MinGW binaries相同版本号的MSYS MinGW，本文安装的Qt MinGW binaries是11.2.0（rev3）版本，参考[mingw-builds-binaries](https://github.com/niXman/mingw-builds-binaries/releases)），然后执行
```bash
pacman -S mingw-w64-x86_64-gsl
```
输出
>正在解析依赖关系...
>正在查找软件包冲突...
>
>软件包 (1) mingw-w64-x86_64-gsl-2.7.1-2
>
>下载大小：       2.03 MiB
>全部安装大小：  12.85 MiB
>
>:: 进行安装吗？ [Y/n] Y
>:: 正在获取软件包......
> mingw-w64-x86_64-gsl-2.7.1-2-any              2.0 MiB  1256 KiB/s 00:02 [#######################################] 100%
>(1/1) 正在检查密钥环里的密钥                                             [#######################################] 100%
>(1/1) 正在检查软件包完整性                                               [#######################################] 100%
>(1/1) 正在加载软件包文件                                                 [#######################################] 100%
>(1/1) 正在检查文件冲突                                                   [#######################################] 100%
>(1/1) 正在检查可用存储空间                                               [#######################################] 100%
>:: 正在处理软件包的变化...
>(1/1) 正在安装 mingw-w64-x86_64-gsl                                      [#######################################] 100%

假设MSYS2的安装目录为D:\Program_Files\msys64，Qt的安装目录为D:\Qt，则将D:\Program_Files\msys64下的bin\libgsl-27.dll、bin\libgslcblas-0.dll、include\gsl、lib\libgsl.a、lib\libgsl.dll.a、lib\libgslcblas.a、lib\libgslcblas.dll.a分别复制到D:\Qt\Tools\mingw1120_64下的对应bin、include、lib目录下。
在项目的CMakeLists.txt中添加
```cmake
find_package(GSL REQUIRED)
```
在`target_link_libraries()`语句中添加`GSL::gsl`：
```cmake
target_link_libraries(YourApp PRIVATE
    Qt6::Core
    Qt6::Gui
    Qt6::Qml
    Qt6::Quick
    GSL::gsl
)
```
在`main.cpp`中添加测试：
```cpp
#include "gsl/gsl_sf_bessel.h"
#include <iostream>
```
（若在Qt Creator中编辑且已正确配置，那么Qt Creator会成功提示gsl的头文件）。在`main()`函数中添加：
```cpp
    double x=10.0;
    double y=gsl_sf_bessel_J0(x);
    std::cout<<"J0("<<x<<")="<<y<<std::endl;
```
按Ctrl+R运行，在3 应用程序输出中查看：
>J0(10)=-0.245936

运行成功。