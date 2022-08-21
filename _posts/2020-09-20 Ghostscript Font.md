---
title: Ghostscript已有字体报错can‘t find font file问题的原因
categories: CSDN补档
tags:
  - ghostscript
  - postscript
abbrlink: 41027
date: 2020-09-20 21:59:59
---

![img](2020-09/20200920215819386.png)

执行例如

```
loadallfonts
```

 命令，报错的字体均为Ghostscript已安装字体，解决方法：

检查Ghostscript安装目录gs\gs9.53.1\Resource\Font，删掉非预装字体。