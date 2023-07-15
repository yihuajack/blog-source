---
title: LaTeX minted报错Package minted Error: Missing Pygments output
categories: LaTeX
tags:
  - latex
  - minted
  - pip
date: 2023-05-30 17:15:29
---

[#【5.30投稿可获定制勋章，仅限今天】全国科技者工作日—为创新和未来而努力#](https://activity.csdn.net/creatActivity?id=10443)

在确认已使用`pip install Pygments`命令安装好`Pygments`包并在 TEX 文件的开头添加了

```latex
\usepackage[utf8]{inputenc}
\usepackage[english]{babel}
\usepackage[cache=false]{minted}
```
后，使用`pdflatex -shell-escaped WA1.tex`命令编译 TEX 文件报错：
>(./_minted-WA1/default-pyg-prefix.pygstyle) (./_minted-WA1/emacs.pygstyle)'pygmentize'      ڲ    ⲿ   Ҳ   ǿ    еĳ   
>   ļ   
> [5 <./5.jpg>]
>
>f:/Documents/GitHub/ECE6703J_SU2023/WA1/WA1.tex:218: Package minted Error: Missing Pygments output; \inputminted was
>probably given a file that does not exist--otherwise, you may need 
>the outputdir package option, or may be using an incompatible build tool,
>or may be using frozencache with a missing file.
>
>See the minted package documentation for explanation.
>Type  H \<return>  for immediate help.
> ...                                              
>
>l.218 \end{minted}
>
>[6] (./WA1.aux) )
>(see the transcript file for additional information){d:/texlive/2021/texmf-dist/fonts/enc/dvips/cm-super/cm-super-ts1.enc}<d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmbx10.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmbx12.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmmi10.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmmi5.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmmi7.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmr10.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmr12.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmr5.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmsy10.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/amsfonts/cm/cmsy7.pfb><d:/texlive/2021/texmf-dist/fonts/type1/public/cm-super/sfrm1000.pfb>
>Output written on WA1.pdf (6 pages, 267083 bytes).
>SyncTeX written on WA1.synctex.gz.

经过排查，此错误是由于安装 Python 包`Pygments`时安装在了非`PATH`路径下导致的。运行`pip install Pygments`输出
>Requirement already satisfied: Pygments in c:\users\\<username\>\appdata\roaming\python\python311\site-packages (2.14.0)

简单直接的解决办法就是将该路径的上一级目录（C:\Users\Yihua\AppData\Roaming\Python\Python311\Scripts）添加到`PATH`，注意到该路径下存在着我们需要的可执行文件`pygmentize.exe`。添加并刷新环境变量后，在 PowerShell 运行
```powershell
pygmentize -V
```
输出
>Pygments version 2.14.0, (c) 2006-2022 by Georg Brandl, Matthäus Chajdas and contributors.

重新编译 TEX 文件即可成功。