---
title: Xilinx Vitis 2021 Cannot create listening port tcp:127.0.0.1:3121: Socket bind error
categories: CE
tags:
  - xilinx
  - vitis
  - xsct
  - sdsoc
  - socket
date: 2021-10-12 14:16:19
---

In Xilinx Vitis 2021, Menu->Xilinx->Program Device:

![img](2021-10/2021101214091269.png)

It showed an error:

![img](2021-10/2021101214093137.png)

I referred to 
67426 - SDSoC - Cannot debug application when targeting a custom platform (xilinx.com)
https://support.xilinx.com/s/article/67426?language=en_US

but it is not applicable for Xilinx Vitis 2021. Inspired by
【分享】如何使用coresight作为MPSoC的标准输入输出？ - HankFu - 博客园 (cnblogs.com)
https://www.cnblogs.com/hankfu/p/14441851.html

I opened Menu->Xilinx->XSCT Console and ran

```
connect
```

![img](2021-10/20211012141323148.png)

I ran

```bash
netstat -ano | findstr 3121
```

but found nothing. Finally, I closed Xilinx Vivado on my computer and tried to reconnect, it succeeded:

![img](2021-10/20211012141459713.png)

Then, re-program device, anything would be OK.

Edit: I later found that the problem may occur even after closing Vivado. I guess it may related to other applications running on my computer, such as proxy applications. Anyway, restarting the computer always works.
