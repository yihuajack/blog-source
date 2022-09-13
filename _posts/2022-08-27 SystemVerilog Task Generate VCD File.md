---
title: SystemVerilog 任务生成 VCD 文件
categories: CE
tags:
  - systemverilog
  - vcd
  - evcd
  - dump
date: 2022-08-27 09:38:42
---

本文原载 [System Verilog tasks to generate vcd file](http://munjalm.blogspot.com/2019/09/system-verilog-tasks-to-generate-vcd.html)，原作者 [Munjal The Mystery](https://www.blogger.com/profile/17631271385913082528)，原作日期 2019 年 9 月 2 日 11 时 57 分 + 5 时 30 分。
在验证活动中，很多时候都需要生成 VCD 文件，设计人员需要查看设计变量的值的变化情况。通过 SystemVerilog可以生成 2 种类型的 VCD 文件：

1. 4-state：将 Value Change 表示为 0，1，X，和 Z。
2. Extended：除了 4-state 值外，还显示强度信息

- 生成 4-state VCD 文件：
  - `$dumpfile("file_name.vcd")` => 生成指定名称的 vcd 文件。它作为参数传递。如果没有提供，则默认文件名为“dump.vcd”。
  - `$dumpvars(0, top)` => 指定将哪些变量转储到 vcd 文件中。它有两个参数，如果没有指定参数，它会转储设计的所有变量。

  第一个参数表示要转储的每个指定模块实例下面的层次结构级别。
  - 0 表示转储指定模块中的所有变量以及当前模块中指定的所有模块实例和子实例。
  - 1 表示转储当前模块的所有变量
  - 2 表示转储当前模块的所有变量和当前模块中指定的实例。它不会转储子模块信息。
  - ...

  第二个参数指示要转储的设计的范围/层次结构。
  - `$dumpoff` => 暂停转储信息到 vcd 文件。
  - `$dumpon` => 恢复转储信息到 vcd 文件。
  - `$dumpall` => 在调用此任务时转储所有选中的变量，否则只有当变量的值发生变化时才转储变量。
  - `$dumplimit(file_size_in_bytes)` => 指定 vcd 文件的大小限制。
- 生成 Extended VCD 文件：
  - `$dumpports(module_identifier, "file_name.vcd")` => 转储端口并指定 vcd 文件名。只能提供模块，不能提供变量。可以通过逗号分隔提供多个模块。
  - `$dumpportsoff("file_name.vcd")` => 暂停转储端口信息到 vcd 文件。
  - `$dumpportson("file_name.vcd")` => 恢复转储端口信息到 vcd 文件。
  - `$dumpportsall("file_name.vcd")` => 在调用此方法时一次性转储所有选定的端口。
  - `$dumpportslimit(file_size_in_bytes, "file_name.vcd")` => 指定要转储到的 vcd 文件的大小限制和文件名。

