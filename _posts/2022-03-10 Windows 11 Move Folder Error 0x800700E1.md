---
title: Windows 11 移动文件夹错误 0x800700E1 无法成功完成操作
categories: SuperUser
tags:
  - windows
  - windows 11
  - 系统安全
  - 安全
date: 2022-03-10 17:10:26
---

Windows 11 移动文件夹时有部分文件弹出窗口：错误 0x800700E1 无法成功完成操作。以下为解决方法（对其他操作也有效）：

设置->隐私和安全性->安全性->Windows 安全中心：

![img](2022-03/f728265b731a409a801dd2afc07e6886.png)

打开 Windows 安全中心->病毒和威胁防护或直接点击保护区域->病毒和威胁防护：

![img](2022-03/aa2777695897410ebf1819cf93903bcd.png)

“病毒和威胁防护”设置->管理设置：
![img](2022-03/74c63519d3b24c4c97f93678bec9067a.png)

排除项->添加或删除排除项：
![img](2022-03/4ba6f2f435fb4bac9e97c656466363f5.png)

添加排除项：
![img](2022-03/98356f078f604232bb2cf81e35a53195.png)

将出现问题的文件或文件夹添加即可。
