---
title: >-
  使用WSL2/Ubuntu配置CLion出现Credentials are not valid for this WSL
  distribution问题的解决方法
categories: CSDN补档
tags:
  - CLion
  - WSL
  - WSL2
abbrlink: 1944
date: 2019-12-21 14:14:32
---

在根据JetBrains官方脚本设置好WSL2/Ubuntu的SSH后打开CLion->Settings->Build, Execution, Deployment->Toolchains将Environment设置为WSL路径为C:\Users\<username>\AppData\Local\Microsoft\WindowsApps\ubuntu.exe可被识别，然后设置Credentials ssh://<username>@localhost:2222发现WSL CMake和WSL GDB可被正常识别，但无法找到Make、C Compiler和C++ Compiler，并出现Credentials are not valid for this WSL distribution (Ubuntu)报错。打开CLion->Help->Show Log in Explorer，然后Help->Debug Log Settings的窗口中输入#com.jetbrains.cidr.cpp.toolchains.WSL关闭CLion，删除Log文件夹中的idea.log，重新打开CLion并打开Toolchains设置直到其完成环境检测，打开Log文件发现警告

20xx-xx-xx xx:xx:xx,xxx [ xxxxxx]  WARN -   #com.intellij.execution.wsl - WSL rootfs doesn't exist: C:\Users\<username>\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs。

事实上我们知道路径C:\Users\<username>\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState中原先WSL1是显示内部各文件夹的，在WSL2中高度虚拟化，变成了一个vhdx硬盘映像文件，所以CLion无法找到内部文件夹rootfs（WSL根目录），所以会报错。到目前（CLion 2019.2版本）CLion尚不支持WSL2，解决方法是在Ubuntu中输入wsl --set-version Ubuntu> 1将WSL版本倒退回1使用。