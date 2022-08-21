---
title: Cygwin快速配置C++环境+ZSH
categories: CSDN补档
tags:
  - cygwin
  - zsh
abbrlink: 1017
date: 2020-08-23 01:05:17
---

1. [Cygwin官网](http://www.cygwin.com/)下载[setup-x86_64.exe](http://www.cygwin.com/setup-x86_64.exe)并安装。选择Install from Internet、Use System Proxy Settings，在User URL中Add http://mirrors.aliyun.com/cygwin/。

2. 安装gcc-core、gcc-g++、make、gdb、binutils、python38、cmake、man-db、nano、openssl、libssl1.1、libstdc++6、vim、wget、xz、curl、git、perl、openssh、mintty、zsh等。

3. /etc/bash.bashrc添加

   ```bash
   HOME="/cygdrive/d/Documents/Programming/cygwinhome"
   PATH="/usr/local/bin:/usr/bin"
   cd
   exec zsh
   ```

   /etc/nsswitch.conf取消db_home注释并改为

   ```bash
   db_home:  /cygdrive/d/Documents/Programming/cygwinhome
   ```

   /etc/profile添加

   ```bash
   HOME="/cygdrive/d/Documents/Programming/cygwinhome"
   ```

   将.zshrc放在D:\Documents\Programming\cygwinhome中，改为

   ```bash
   export ZSH="/cygdrive/d/Program_Files/.oh-my-zsh"
   ZSH_THEME="agnoster"
   ```

   参考[How can I change my Cygwin home folder after installation?](https://stackoverflow.com/questions/1494658/how-can-i-change-my-cygwin-home-folder-after-installation)、https://cygwin.com/cygwin-ug-net/ntsec.html#ntsec-mapping-nsswitch-syntax及[Cygwin如何更改HOME目录（MSYS2和Cygwin ZSH冲突）](https://blog.csdn.net/yihuajack/article/details/105249209)。

4. Cygwin Terminal窗口Options->Text->Font更改字体。

5. （可选）Windows Terminal配置：settings.json中的"profiles"下的"list"下添加

   ```
   {
       "guid": "{3bf9cb4f-ac0a-49ee-8002-f11a648c0ea2}",
       "name": "Cygwin64",
       "commandline": "D:\\Program_Files\\cygwin64\\bin\\bash.exe",
       "icon": "D:\\Program_Files\\cygwin64\\Cygwin.ico"
   }
   ```

   （可选）ConEmu配置：Settings->General->Fonts更改字体；Settings->Startup->Tasks，{Bash:CygWin bash}中Commands默认为

   ```
   set CHERE_INVOKING=1 & set "PATH=%ConEmuDrive%\Program_Files\cygwin64\bin;%PATH%" & %ConEmuBaseDirShort%\conemu-cyg-64.exe -new_console:p %ConEmuDrive%\Program_Files\cygwin64\bin\bash.exe --login -i -new_console:C:"%ConEmuDrive%\Program_Files\cygwin64\Cygwin.ico"
   ```

   （可选）Fluent Terminal设置：配置文件->新建。名称：Cygwin；可执行程序位置：D:\Program_Files\cygwin64\bin\bash.exe；工作目录：D:\Documents\Programming\cygwinhome；快捷键：自选。