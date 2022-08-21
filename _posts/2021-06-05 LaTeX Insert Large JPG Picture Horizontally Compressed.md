---
title: LaTeX插入大尺寸JPG图片出现纵横比异常水平被压缩的解决方法
categories: Other Language
tags:
  - latex
date: 2021-06-05 12:57:57
---

LaTeX中使用\includegraphics[width=\textwidth]{picname.jpg}插入一个约5 MiB大小的图片时，图LaTeX使用\includegraphics[width=\textwidth]{*.jpg}插入一张大小约为5 MiB的JPG格式图片，发现图片在水平方向上被压扁，纵横比远大于原来的纵横比，从而显示异常，解决方法为：

用Windows画图将JPG图片另存为PNG图片，改为插入该PNG图片即可解决问题。

该问题可能与使用Windows图片的旋转图片功能旋转JPG图片有关。
