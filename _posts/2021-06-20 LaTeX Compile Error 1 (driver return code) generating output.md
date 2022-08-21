---
title: LaTeX编译报错Error 1 (driver return code) generating output的可能原因
categories: Other Language
tags:
  - latex
date: 2021-06-20 23:55:26
---

在边写边编译一个LaTeX文件时突然编译失败，试图删掉一些最近添加的内容后能编译成功，但是在边写边编译一个LaTeX文件时突然编译失败，试图先删除一些最近添加的内容，编译成功，但慢慢把添加的内容加回来则会编译失败，检查加回来的内容没有问题，查看日志文件没有任何明确的报错信息，仅在结尾处显示

> \*geometry* verbose mode - [ newgeometry ] result:
> \* driver: xetex
> \* paper: a4paper
> \* layout: <same size as paper>
> \* layoutoffset:(h,v)=(0.0pt,0.0pt)
> \* modes: 
> \* h-part:(L,W,R)=(36.13512pt, 525.23764pt, 36.13512pt)
> \* v-part:(T,H,B)=(36.13512pt, 772.77661pt, 36.13512pt)
> \* \paperwidth=597.50787pt
> \* \paperheight=845.04684pt
> \* \textwidth=525.23764pt
> \* \textheight=772.77661pt
> \* \oddsidemargin=-36.13487pt
> \* \evensidemargin=-36.13487pt
> \* \topmargin=-73.13487pt
> \* \headheight=12.0pt
> \* \headsep=25.0pt
> \* \topskip=10.0pt
> \* \footskip=30.0pt
> \* \marginparwidth=57.0pt
> \* \marginparsep=11.0pt
> \* \columnsep=10.0pt
> \* \skip\footins=9.0pt plus 4.0pt minus 2.0pt
> \* \hoffset=0.0pt
> \* \voffset=0.0pt
> \* \mag=1000
> \* \@twocolumnfalse
> \* \@twosidefalse
> \* \@mparswitchfalse
> \* \@reversemarginfalse
> \* (1in=72.27pt=25.4mm, 1cm=28.453pt)
>
> (./Assignment3_B.txt [9
>
> ] [10])
> File: 1.eps Graphic file (type eps)
> <1.eps>
> File: 2.eps Graphic file (type eps)
> <2.eps>
> File: 3.eps Graphic file (type eps)
> <3.eps>
>  [11] [12] (./Assignment 3.aux) ) 
> Here is how much of TeX's memory you used:
>  13837 strings out of 476919
>  294415 string characters out of 5825424
>  870716 words of memory out of 5000000
>  33593 multiletter control sequences out of 15000+600000
>  413549 words of font info for 81 fonts, out of 8000000 for 9000
>  1348 hyphenation exceptions out of 8191
>  85i,11n,93p,406b,1541s stack positions out of 5000i,500n,10000p,200000b,80000s
>
> Error 1 (driver return code) generating output;
> file Assignment 3.pdf may not be valid.

等内容。后来发现C盘空间满了，于是清理C盘清除约1个G的空间后重新编译即成功。
