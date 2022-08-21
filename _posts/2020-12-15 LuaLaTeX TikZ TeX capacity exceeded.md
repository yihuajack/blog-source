---
title: LaTeX使用LuaLaTeX和TikZ编译时出错TeX capacity exceeded, sorry [input stack size=5000]
categories: Other Language
tags:
  - latex
  - tikz
  - lualatex
date: 2020-12-15 21:56:53
---

根据[Tex capacity exceeded, sorry input stack size=5000](https://tex.stackexchange.com/questions/410121/tex-capacity-exceeded-sorry-input-stack-size-5000)，该问题可能由于\end{document}没加引起，但检查后发现\end{document}并没有缺失。

根据[TeX capacity exceeded, sorry](https://github.com/matlab2tikz/matlab2tikz/wiki/TeX-capacity-exceeded,-sorry)，该问题可通过改用LuaLaTeX解决，然而一开始就用的LuaLaTeX；根据MikTeX或TeX Live/MacTeX的不同修改配置增加容量，无效。

根据[【漫漫科研路\pgfplots】克服绘制色温图时，数据量大出现的内存限制](https://blog.csdn.net/tengweitw/article/details/103657253)增加配置修改容量，无效。

最后检查是报错的地方的前一个tikzpicture没有加\end{tikzpicture}，添加后问题解决。