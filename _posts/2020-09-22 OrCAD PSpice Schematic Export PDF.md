---
title: OrCAD PSpice原理图Schematic导出PDF没反应的解决方法
categories: CSDN补档
tags:
  - orcad
  - pspice
abbrlink: 12875
date: 2020-09-22 21:18:31
---

首先确定E:\Program Files\Adobe\Acrobat DC\Acrobat和D:\Program Files\gs\gs9.53.1\bin已被添加到环境变量。

选中要打印的页面：

![img](2020-09/20200922211431160.png)

点击菜单栏File->Export->PDF，修改文件名，确认Converter和Converter Path，其余选项配置保持默认即可：

![img](2020-09/20200922211552971.png)

或

![img](2020-09/2020092221161535.png)

即可成功导出。Acrobat Distiller和GhostScript的不同之处在于前者会创建一个同名的.log文件。

如果选中要导出的页面后仍然没反应，则点击左侧工程管理器Project名称或Design名称后再Export。