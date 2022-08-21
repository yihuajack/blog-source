---
title: LaTeX中align和alignat之间的区别
categories: CSDN补档
tags: latex
abbrlink: 57768
date: 2020-03-22 15:33:43
---

第一个区别是使用align不需要额外的参数：

```
\begin{align}
```

而alignat需要：

```
\begin{alignat}{<number>}
```

两种环境都创建基于rl对的列对齐，align会根据你写的内容创建足够多的列，而alignat需要你提前指定想要多少列。

然而，这两种环境还有更多不同。align环境会在列与列之间添加水平空格

```
<r col><l col> <space> <r col><l col> <space> <...>
```

而alignat不会添加水平空格。例如见[Align-environment: Align on the left side](https://tex.stackexchange.com/questions/200502/align-environment-align-on-the-left-side)中的一个对齐

```
<l col><l col>
```

可以通过使用空的右对齐列：

```
\begin{alignat}{2}
&ABC  &&= AB
&ABCD &&= ABC - ABCDEFG
\end{alignat}
```

两种环境在cell的开始处的左对齐列都有一个隐式的{}来使得起始于关系符或运算符的cell能够有合适的间距。

alignat的其他用法包括当我们想要更好地控制列与列之间的水平间距时。这样的间距应当被显式地指定如下：

```
\begin{alignat*}{3}
& m   \quad && \text{módulo}            \quad && m>0\\
& a   \quad && \text{multiplicador}     \quad && 0<a<m\\
& c   \quad && \text{constante aditiva} \quad && 0\leq c<m\\
& x_0 \quad && \text{valor inicial}     \quad && 0\leq x_0 <m
\end{alignat*}
```

(见[Aligning equations with text with alignat](https://tex.stackexchange.com/questions/49014/aligning-equations-with-text-with-alignat))这样的控制是不可能通过align完成的，因为它总是在列与列之间添加相同长度的水平间距。

两种环境都有*的形式并对于任意行都接受\tag或\notag。

此外还有“内部”版本的aligned和alignedat，在行内公式、显示公式和数学对齐等内部数学模式中它们遵循相同的规则。