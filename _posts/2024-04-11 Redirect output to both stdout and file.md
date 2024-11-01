---
title: Bash将输出同时重定向到标准输出stdout和文件
categories: Linux
tags:
  - bash
  - 服务器
  - 开发语言
date: 2024-04-11 15:17:37
---

本文参考[How to redirect output to a file and stdout](https://stackoverflow.com/questions/418896/how-to-redirect-output-to-a-file-and-stdout)。
对于任意原本默认输出到标准输出`stdout`的程序或命令`foo`，只需执行
```bash
foo | tee output.file
```
即可同时输出到`output.file`文件。
例如，若想输出当前目录下的所有目录与文件到标准输出`stdout`的同时保存到`output.file`文件，执行
```bash
ls -a | tee output.file
```
如果同时想输出程序或命令的标准错误`stderr`到标准输出和文件，只需添加`2>&1`即可：
```bash
program [arguments...] 2>&1 | tee outfile
```
`2>&1`的含义是将 channel 2（标准错误`stderr`）重定向到 channel 1（标准输出`stdout`），这样标准输出和标准错误的内容都将输出到标准输出。
如果想将输出内容附加到`outfile`文件（而非覆写），只需添加`-A`参数：
```bash
program [arguments...] 2>&1 | tee -a outfile
```