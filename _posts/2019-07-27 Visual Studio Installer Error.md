---
title: 关于Visual Studio Installer无法识别已安装的VS并报错找不到路径的问题
tags: Visual Studio Installer Error
categories: CSDN补档
abbrlink: 23359
date: 2019-07-27 17:56:50
---

1. 千万不要删除下载缓存或将下载缓存或安装目录移动到其他位置，如果磁盘空间有限就在安装时将下载缓存放在移动硬盘上。安装后亲测结果当移动硬盘未连接时VS Installer在已安装界面显示为空（即无法识别已安装的VS），插入安装时放下载缓存的移动硬盘后VS Installer重新识别出已安装的VS。

2. 更改默认下载安装路径时最好只改磁盘分区，保留默认的目录树（如D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise）。