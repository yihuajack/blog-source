---
title: 如何安全更改Windows 10用户文件夹名称
categories: CSDN补档
tags: 用户文件夹
abbrlink: 61472
date: 2019-10-29 14:41:36
---

经查网上一些教程，用户文件夹中并没有desktop.ini文件，只得修改注册表值。进入注册表项HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList，在其中项名为S-1-5-21-xxx的两个项中找到ProfileImagePath值更改用户文件夹名称，然后重新启动会发现无法登录到原来的Microsoft账户，这时将环境变量中所有在用户文件夹中的系统变量和用户变量的用户文件夹名称全部更改，检查文件资源管理器中用户文件夹名称是否已更改，若没有也可以发现这个文件夹名称可以直接重命名更改，然后注销账户并重新登录，黑屏一段时间后（其实是任务栏设置被重置成非锁定状态，可调出开始菜单以调出任务栏进行设置）文件资源管理器和桌面启动，这时Windows 邮件、Microsoft Outlook、AutoCAD、COMSOL Multiphysics等应用会无法正常工作，OriginLab等应用可在启动后重新设置配置文件而正常工作，而SolidWorks、Autodesk Inventor、ArcGIS、Cadence OrCAD、Gaussian、Altium Designer、NI Multisim、Office其他软件、Adobe系列、MATLAB、Mathematica、IBM SPSS等均可正常工作。这时使用Registry Toolkit或Registry Workshop批量将"Users\原用户名"全部替换为"Users\现用户名"，中间会触发反病毒软件系统注册表项被修改的提醒略过即可，再重新搜索一遍，将遗漏的"Users\原用户名"替换掉，注意把软件注册信息等注册表留下。操作后只有少数用户配置会被重置回默认值。COMSOL如果还有无法创建目录的问题，将C:\Users\现用户名\AppData\.comsol文件夹中的文件夹删掉重新启动COMSOL即可