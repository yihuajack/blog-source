---
title: WSL无法删除.git文件夹报错rm: cannot remove ‘/.git/objects/pack/pack-.pack‘: Permission denied的一种解决方法
categories: SuperUser
tags:
  - wsl
  - ubuntu
  - windows
date: 2021-05-22 18:04:18
---

这一问题可能是由于在第一次执行删除操作时，有一个Windows主系统程序如文件资源管理器处于浏览位于C:\Users\<username>\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc下的相关文件夹的状态，导致该文件夹无法被删除，即使使用sudo命令用root权限也无法删除。一般问题往往是由于在Windows文件资源管理器中直接对WSL文件系统进行操作导致的，也会导致在Windows文件资源管理器无法删除WSL文件系统的某个文件或文件夹的错误，这种错误一般比较明显，而上述问题则比较隐蔽。显然，该问题只会在WSL 1中出现。无论是在WSL中无法删除还是在Windows中无法删除，都需要重启Windows系统。如果是WSL中无法删除，那么重启后在WSL中再次执行rm删除操作即可成功；如果是在Windows中无法删除，那么重启后会发现要删除的文件或文件夹已经消失。
