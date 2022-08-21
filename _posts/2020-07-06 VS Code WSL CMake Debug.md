---
title: VSCode+WSL+CMake调试
categories: CSDN补档
tags:
  - vscode
  - cmake
  - debug
  - wsl
abbrlink: 26614
date: 2020-07-06 09:38:57
---

1. 确定VSCode、gcc、cmake、gdb、build-essential以及VSCode的插件C/C++和CMake Tools安装正常（在远程窗口中安装）。
2. Ctrl+Shift+P，输入CMake: Quick Start：![img](2020-07/20200706091846675.png)
   如右下角弹出选择窗口选择是即可，然后继续到Select a kit：![img](2020-07/20200706091932745.png)
   如未弹出，则在VSCode底部选择![img](2020-07/2020070609201849.png)
   No Kit Selected即可选择工具。再次CMake: Quick Start：![img](2020-07/20200706092122686.png)
   输入新项目的名称，然后选择Executable：![img](2020-07/20200706092237888.png)
   之后会自动创建并打开CMakeList.txt，完成后：![img](2020-07/20200706092429585.png)
   然后修改CMakeLists.txt，保存会自动配置并在“输出”栏输出cmake配置信息：![img](2020-07/20200706093316888.png)选择CMake: Select Variant：![img](2020-07/20200706092854435.png)
   然后选择Debug：
   ![img](2020-07/2020070609281220.png)
3. Build project：
   ![img](2020-07/20200706093428887.png)
   成功后在输出栏显示[build] Build finished with exit code 0。
4. 像使用launch.json和tasks.json调试一样，设置断点后Debug project：
   ![img](2020-07/20200706093559349.png)