---
title: LaTeX xcolor使用x11names等选项无法识别报错Package xcolor Error：Undefined color的解决方法
categories: CSDN补档
tags:
  - latex
  - xcolor
abbrlink: 25920
date: 2020-09-24 00:04:42
---

例如在使用tikz的node时设定颜色：color=FireBrick2，报错

```
Package xcolor Error: Undefined color `tikz@color'.
 
See the xcolor package documentation for explanation.
Type  H <return>  for immediate help.
 ...                                              
                                                  
l.56     \end{axis}
```

在tex文件开头添加

```
\usepackage[x11names]{xcolor}
```

无效。根据[Package xcolor Error: Undefined colors “Maroon”/“Royal Blue” when master has PDF included classicthesis 3.1](https://tex.stackexchange.com/questions/124636/package-xcolor-error-undefined-colors-maroon-royal-blue-when-master-has-pdf)将x11names添加到documentsclass的选项中即可解决该问题：

```
\documentclass[a4paper,x11names]{article}
\usepackage{xcolor}
```