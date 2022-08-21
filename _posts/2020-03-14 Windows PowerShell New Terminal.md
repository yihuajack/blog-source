---
title: 为Windows Terminal配置新的Shell与默认Shell（以PowerShell Core 7为例）
categories: CSDN补档
tags: powershell
abbrlink: 19739
date: 2020-03-14 22:29:45
---

Windows Terminal默认打开的是系统自带的PowerShell（Windows 10是PowerShell 5.1），而如果想配置一个新的Shell如PowerShell Core 7，则可以按如下操作进行：

1. Ctrl+,打开Settings，我们为PowerShell Core 7先生成一个GUID，比如用[Online GUID Generator](https://www.guidgenerator.com/online-guid-generator.aspx)，然后在打开的profiles.json中的"profiles":"list":中新建一个大括号（与其他Shell平行）：

```
{
    "guid": "{~}",
    "name": "PowerShell Core",
    "commandline": "D:\\Program Files\\PowerShell\\7\\pwsh.exe",
    "acrylicOpacity": 0.5,
    "fontFace": "DejaVu Sans Mono for Powerline",
    "hidden": false,
    "useAcrylic": true
}
```

其中，~为你生成的GUID，name可以自己设置，commandline设置为你安装PowerShell Core的路径，此外的可选项分别为：

useAcrylic: True为使用亚克力效果；acrylicOpacity为亚克力效果的透明度，fontFace为字体设置，DejaVu Sans Mono for Powerline为PowerShell的一个推荐字体。

关于设置默认Shell，只需在profiles.json中的"defaultProfile" “~”中的~替换为PowerShell Core设定的GUID值即可。

Reference:

[如何给 Windows Terminal 增加一个新的终端（以 Bash 为例）](https://msd.misuland.com/pd/3545776840385760818)

[How To Add PowerShell v7 Preview to the New Windows Terminal](https://briantjackett.com/2019/06/24/windows-terminal-with-powershell-v7-preview/)

[Win10 Terminal更换默认Shell](https://blog.csdn.net/littlehaes/article/details/101514339)

[guid在线生成工具](https://www.cnblogs.com/lsysunbow/archive/2012/08/09/2629952.html)