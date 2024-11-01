---
title: WSL2打开x11应用报错Error: Can‘t open display: :0
categories: Environment
tags:
  - wsl
  - x11
  - wslg
date: 2024-10-31 20:27:40
---

Environment：
WSL 版本： 2.3.24.0
内核版本： 5.15.153.1-2
WSLg 版本： 1.0.65
MSRDC 版本： 1.2.5620
Direct3D 版本： 1.611.1-81528511
DXCore 版本： 10.0.26100.1-240331-1435.ge-release
Windows 版本： 10.0.22631.4391
在安装`x11-apps`后打开`xeyes`等 X11 应用报错：
>Error: Can't open dislay: :0

检查`DISPLAY`变量：`echo $DISPLAY`
显示默认值 :0
将`DISPLAY`变量改为`/etc/resolv.conf`文件中的`nameserver`字段或 Windows 主机 WLAN 的 IPv4 地址（即 route.exe print | grep 0.0.0.0）后面加 :0依旧无效，这是因为新 WSL2 的 WSLg 的 X 服务器运行在 display 0 上，所以默认值 :0 就是正确的。`wsl --update`显示为最新版本，并且重启 WSL 无法解决问题。
解决方法：参考[X11 display socket](https://github.com/microsoft/wslg/wiki/Diagnosing-%22cannot-open-display%22-type-issues-with-WSLg#x11-display-socket)
执行
```bash
sudo rm -r /tmp/.X11-unix
ln -s /mnt/wslg/.X11-unix /tmp/.X11-unix
```
重新创建 .X11-unix 的链接即可。