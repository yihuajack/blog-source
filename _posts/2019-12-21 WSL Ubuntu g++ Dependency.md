---
title: 初配置WSL/Ubuntu时apt-get install g++等时出现依赖版本过高（Depends：but it is not going）的问题的解决方法
categories: CSDN补档
tags: 
  - WSL
  - Ubuntu
abbrlink: 54448
date: 2019-12-21 12:57:21
---

不要尝试一个个将依赖版本倒退回指定版本，因为根本没有用， 纯属浪费时间。不要使用网络上提供的源，该问题由于源的问题导致的（如某aliyun源的版本代号是xenial，属16.04LTS版本，在18.04LTS版本上将出现问题，18.04LTS对于的是bionic）。建议到https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/中选定你所安装的系统版本的源更换。换好后记得sudo apt-get update然后sudo apt-get upgrade。这时再执行sudo apt-get install将没有问题。事实上，该问题还会导致SSL配置错误。