---
title: 编译构建Linux内核模块错误classmap.h: No such file or directory
categories: SuperUser
tags:
  - linux
  - ubuntu
  - kbuild
  - kernel
date: 2021-11-14 00:23:14
---

执行

```bash
sudo make -C /usr/src/linux-headers-5.11.0-1021-raspi SUBDIRS=$PWD modules
make: Entering directory '/usr/src/linux-headers-5.11.0-1021-raspi'
```

报错

> make: Entering directory '/usr/src/linux-headers-5.11.0-1021-raspi'
>   SYNC    include/config/auto.conf.cmd
>   YACC    scripts/kconfig/parser.tab.[ch]
>   HOSTCC  scripts/kconfig/lexer.lex.o
>   HOSTCC  scripts/kconfig/parser.tab.o
>   HOSTCC  scripts/kconfig/preprocess.o
>   HOSTCC  scripts/kconfig/symbol.o
>   HOSTCC  scripts/kconfig/util.o
>   HOSTLD  scripts/kconfig/conf
>   HOSTCC  scripts/dtc/dtc.o
>   HOSTCC  scripts/dtc/flattree.o
>   HOSTCC  scripts/dtc/fstree.o
>   HOSTCC  scripts/dtc/data.o
>   HOSTCC  scripts/dtc/livetree.o
>   HOSTCC  scripts/dtc/treesource.o
>   HOSTCC  scripts/dtc/srcpos.o
>   HOSTCC  scripts/dtc/checks.o
>   HOSTCC  scripts/dtc/util.o
>   LEX     scripts/dtc/dtc-lexer.lex.c
>   YACC    scripts/dtc/dtc-parser.tab.[ch]
>   HOSTCC  scripts/dtc/dtc-lexer.lex.o
>   HOSTCC  scripts/dtc/dtc-parser.tab.o
>   HOSTLD  scripts/dtc/dtc
>   HOSTCC  scripts/genksyms/genksyms.o
>   YACC    scripts/genksyms/parse.tab.[ch]
>   HOSTCC  scripts/genksyms/parse.tab.o
>   LEX     scripts/genksyms/lex.lex.c
>   HOSTCC  scripts/genksyms/lex.lex.o
>   HOSTLD  scripts/genksyms/genksyms
>   HOSTCC  scripts/selinux/genheaders/genheaders
> scripts/selinux/genheaders/genheaders.c:18:10: fatal error: classmap.h: No such file or directory
>    18 | #include "classmap.h"
>       |          ^\~\~\~\~\~\~\~\~\~\~\~
> compilation terminated.
> make[3]: \*** [scripts/Makefile.host:95: scripts/selinux/genheaders/genheaders] Error 1
> make[2]: \*** [scripts/Makefile.build:519: scripts/selinux/genheaders] Error 2
> make[1]: \*** [scripts/Makefile.build:519: scripts/selinux] Error 2
> make: *** [Makefile:1225: scripts] Error 2
> make: Leaving directory '/usr/src/linux-headers-5.11.0-1021-raspi'

参考 [Bug #1895470 “fatal error: classmap.h: No such file or directory...” : Bugs : linux-kernel-headers package : Ubuntu](https://bugs.launchpad.net/ubuntu/+source/linux-kernel-headers/+bug/1895470) theway (lexueye) 的回答，Ubuntu 20.04 起不再支持 SUBDIR=，应改为 M=：

```bash
sudo make -C /usr/src/linux-headers-5.11.0-1021-raspi M=$PWD modules
make: Entering directory '/usr/src/linux-headers-5.11.0-1021-raspi'
```

即可成功编译。
