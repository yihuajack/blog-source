---
title: Git for Windows 2.25.1更改$HOME路径
categories: CSDN补档
tags: git
abbrlink: 36466
date: 2020-03-21 17:45:55
---

由于之前安装Cadence更改了系统环境变量，Git for Windows的$HOME目录定位到了其他地方，新版的Git for Windows在安装目录（如D:\Program_Files\Git）下的/etc/profile中已经删除了HOME相关的设置行，但是不用担心，我们只需要在profile文件开头加入：

```
HOME="C:/Users/<username>"
```

或其他你想指定的目录即可更改$HOME路径，无视环境变量中对HOME的定义。

Reference:

[Git for Windows - custom home directory](https://www.riuson.com/blog/post/git-for-windows-custom-home-directory)