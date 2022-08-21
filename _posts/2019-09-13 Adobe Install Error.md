---
title: 关于安装Adobe CC系列Photoshop等软件时报错无法写入注册表值错误代码160问题的解决方法
tags:
  - Adobe
  - Adobe安装错误
categories: CSDN补档
abbrlink: 9336
date: 2019-09-13 20:04:12
---

几个月前在研究音频处理时试图更新Audition出错，今天更新PS和AI再度出错，于是在C:\Program Files (x86)\Common Files\Adobe\Installers下找到install.log或在C:\ProgramData\Adobe\Installer下找到Summary.htm如下（以Photoshop为例，对Adobe各软件均有效）：

```
Exit Code: 160
-------------------------------------- Summary --------------------------------------
 - 2 fatal error(s), 0 error(s), 3 warnings(s) 

FATAL: Error (Code = 160) executing in command 'SetRegistryValueCommand' for package: 'AdobePhotoshop20-Core_x64', version:20.0.6.80
FATAL: Error occurred in install of package (Name: AdobePhotoshop20-Core_x64 Version: 20.0.6.80). Error code: '160'
WARN: Asset(versioned) File in target location 'C:\Program Files (x86)\Common Files\Adobe\Color\Profiles\Recommended\JapanColor2001Coated.icc' is locked by another application. Skipping replacement
WARN: Asset(versioned) File in target location 'C:\Program Files (x86)\Common Files\Adobe\Color\Profiles\Recommended\USWebCoatedSWOP.icc' is locked by another application. Skipping replacement
WARN: Error Setting Registry - Start 64-bit:1 root:0 key:.cin name:Default type:REG_SZ data:Photoshop.CINFile.130. Check for Registry permissions. (Error: Error 5 鎷掔粷璁块棶銆�)
-------------------------------------------------------------------------------------
```

其中第1、2条warning是由于文件被其他Adobe程序（如Acrobat）占用所致问题不大，看到第3条warning找到注册表值Photoshop.CINFile.130.在regedit或其他注册表管理器中找到该值发现位于HKEY_LOCAL_MACHINE\SOFTWARE\Classes下，将该项下所有关于Photoshop的注册表全部删掉（注意避开其他Adobe软件和Solidworks等软件相关联的注册表），并将Classes的Administrators的权限改为完全控制，并将所有者也改为Administrators，使用可从此对象继承的权限项目替换所有子对象的权限项目，替换子容器和对象的所有者。权限更改后Adobe CC系列各软件的此类问题全部解决，但是可能导致开始菜单桌面应用图标无法显示的问题。