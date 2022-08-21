---
title: PowerShell使用import-module安装oh-my-posh下载慢、卡的问题的解决方法
categories: CSDN补档
tags: powershell
abbrlink: 6240
date: 2020-03-14 20:33:28
---

该方法对Windows自带Powershell（Windows 10是PowerShell 5.1）和新版的Powershell 6、7等版本均有效。

初体验PowerShell 7执行命令

```
Install-Module oh-my-posh -Scope CurrentUser
```

后并按A键确认后停留在Install的界面，抑或是进入Download界面下载0.03MB或0.06MB后卡住不动，在设置代理后仍然如此，在设置

```
Set-ExecutionPolicy Bypass
```

或

```
set-ExecutionPolicy RemoteSigned
```

后仍然如此。但是打开之前已经配置好的Powershell 5.1执行

```
Update-Module
```

后发现其迅速成功更新了oh-my-posh模块，发现我们只需**用管理员方式启动**Powershell 7再安装即可。事实上新版的PowerShell无需像旧版那样配置Set-ExecutionPolicy和NuGet即可顺利安装模块，并且预装了PSReadLine等。