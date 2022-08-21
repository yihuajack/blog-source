---
title: Windows 10 ttf字体文件右键菜单没有安装选项导致MATLAB预设无法识别使用已安装字体的问题的解决方法
categories: CSDN补档
tags:
  - matlab
  - ttf
  - opentype
abbrlink: 18163
date: 2020-02-18 23:30:04
---

最近试图把MATLAB预设字体改成github上的Consolas-with-Yahei字体，clone到本地解压出.ttf字体文件后将其复制到C:\Windows\Fonts自动安装后发现其预设字体下拉栏中并无该字体。在网上寻找无果，经测，在控制面板->外观和个性化->字体->字体设置中允许使用快捷方式安装字体勾选后仍然无法解决，于是打开regedit注册表编辑器进入计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts(*)进行注册表操作，以Consolas-with-Yahei Bold Nerd Font.ttf字体文件为例（保险起见应先关闭MATLAB，并确定该字体文件已被复制到C:\Windows\Fonts并安装成功）：

在(*)目录下新建字符串值名为"Consolas-with-Yahei Bold (TrueType)",将其值输入为"Consolas-with-Yahei Bold Nerd Font.ttf"。

为Consolas-with-Yahei Nerd Font.ttf, Consolas-with-Yahei Italic Nerd Font.ttf, Consolas-with-Yahei Bold Italic Nerd Font.ttf等重复上述操作。重新打开MATLAB预设->字体，发现下拉栏中已有Consolas-with-Yahei字体名字，问题成功解决。

**注意：注册表中注册字体时值的名称必须为在C:\Windows\Fonts中安装的名称！**例如在Fonts目录中安装后显示的名称为："YaHei Consolas Hybrid 常规"：

![img](2020-02/202002190025245.png)

则值名应设为"YaHei Consolas Hybrid (TrueType)"，若设为其他名称则问题依旧存在。

另外，在先前操作中发现右键.ttf字体文件或Windows\Fonts中的字体弹出右键菜单会弹出报错窗口提示Inventor等程序的启动问题接着显示残缺的右键菜单，将Autodesk Inventor卸载后则情况消失，怀疑该问题的出现与Inventor等程序有关，因为此前Inventor的授权被Solidworks、Cadence Allegro等软件干扰导致许可证出现问题使得Inventor等无法启动。