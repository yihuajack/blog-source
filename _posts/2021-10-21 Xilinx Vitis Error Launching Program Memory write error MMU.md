---
title: Xilinx Vitis Error Launching Program: Memory write error MMU section translation fault
categories: CE
tags:
  - xilinx
  - vitis
  - jtag
date: 2021-10-21 23:32:32
---

在 Run As -> Launch Hardware (Single Application Debug (GDB)) 时报错：

Error while launching program:

Memory write error at 0x100000. MMU section translation fault

![img](2021-10/510dcda5cf784a67982e41d511e69728.png)

原因是 JP4 接口误接为 SD，改接为 JTAG 后即可顺利 Program Device。
