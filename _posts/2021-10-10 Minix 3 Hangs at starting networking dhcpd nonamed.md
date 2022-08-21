---
title: Minix 3 系统启动时在 starting networking dhcpd nonamed 处挂起的解决方法
categories: SuperUser
tags:
  - minix
  - dhcp
  - vmware
  - virtualbox
  - nat
date: 2021-10-10 14:56:47
---

该方法至少对 Minix 3.2.0 - 3.2.1，VirtualBox 4.2 - 4.3.12, VMware Workstation 8 - 15.5.7 版本有效。

参考：

virtualbox.org • View topic - [SOLVED] Minix hangs at boot
https://forums.virtualbox.org/viewtopic.php?f=4&t=61975
Re: [minix3] Boot never progresses past dhcpd nonamed in VMware and VirtualBox (google.com)
https://groups.google.com/g/minix3/c/A_fyO5SaNbw
Windows + R 键调出“运行“，输入 services.msc 打开服务，检查 VMware DHCP Service 和 VMware NAT Service 服务是否已启动：

![img](2021-10/20211010143659314.png)

如果未启动，启动这两个服务，重启 VMware Workstation 后即可：

![img](2021-10/20211010145546819.png)
