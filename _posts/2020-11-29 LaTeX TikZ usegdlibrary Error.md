---
title: LaTeX TikZ使用usegdlibrary命令报错Undefined control sequence的原因及解决方案
categories: Other Language
tags:
  - latex
  - tikz
  - vscode
date: 2020-11-29 15:24:57
---

原因：“usegdlibrary”命令基于TikZ库“graphdrawing”，该库需要LuaTeX/LuaLaTeX支持。

解决方案：如已安装Tex Live，以VS Code为例，在settings.json的“latex-workshop.latex.tools”下添加

```json
    {
        "name": "lualatex",
        "command": "lualatex",
        "args": [
            "-shell-escape",
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOC%"
        ]
    }
```
在“latex-workshop.latex.recipes”下添加

```json
    {
        "name": "LuaLaTeX",
        "tools": [
            "lualatex"
        ]
    }
```
点击COMMANDS -> Build LaTeX rpoject -> Recipe: LuaLaTeX编译