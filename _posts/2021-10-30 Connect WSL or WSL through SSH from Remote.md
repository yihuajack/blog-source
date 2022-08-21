---
title: 从远程服务器通过SSH连接WSL或WSL2
categories: SuperUser
tags:
  - ssh
  - 服务器
  - 运维
  - wsl
  - portproxy
date: 2021-10-29 13:28:25
---

方法一（简单方法）：

参考
THE EASY WAY how to SSH into Bash and WSL2 on Windows 10 from an external machine - Scott Hanselman's Blog
https://www.hanselman.com/blog/the-easy-way-how-to-ssh-into-bash-and-wsl2-on-windows-10-from-an-external-machine

首先确认 Windows 的 bash 已经绑定给 WSL/WSL2，然后使用

```powershell
ssh -t username@servername "bash"
```

即可连接。如果要设置为默认，在 PowerShell Core 中执行

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "bash.exe" -PropertyType String -Force
```

即可。

方法二（复杂方法）：

参考

WSL 2 Networking
https://youtu.be/yCK3easuYm4
在 WSL 2 中执行

```bash
ip addr | grep eth0
```

得到 WSL 2 的 IP 地址，然后以管理员身份运行 PowerShell Core，执行

```bash
netsh interface portproxy add v4tov4 listenport=3390 listenaddress=0.0.0.0 connectport=3390 connectaddress=<wsl2_ip_addr>
```

然后为端口 3390 设置 Windows Defeder 防火墙入站规则允许通过。

WSL2 的 IP 地址例如 172.23.239.4/20 连接到 Windows 的 Ethernet adapter vEthernet (WSL) 的 IP 地址例如 172.23.224.1/20，再通过 Wireless LAN adapter WLAn 的 IP 地址例如 192.168.1.132/24 连接到 WiFi。

由于 WSL 的 IP 地址每次都会发生改变，所以可以参考
[WSL 2] NIC Bridge mode 🖧 (Has TCP Workaround🔨) · Issue #4150 · microsoft/WSL (github.com)
https://github.com/microsoft/WSL/issues/4150

 使用以下脚本一键配置。

```powershell
$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  echo "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,10000,3000,5000);


#[Static ip]
#You can change the addr to your ip config to listen to a specific address
$addr='0.0.0.0';
$ports_a = $ports -join ",";


#Remove Firewall Exception Rules
iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

#adding Exception Rules for inbound and outbound Rules
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
```

将其保存为一个 ps1 文件，然后以 Execution 为 ByPass 执行

```powershell
wslbridge.ps1 -ExecutionPolicy Bypass
```

参考
How to SSH into WSL2 on Windows 10 from an external machine - Scott Hanselman's Blog
https://www.hanselman.com/blog/how-to-ssh-into-wsl2-on-windows-10-from-an-external-machine
可以用

```powershell
netsh interface portproxy show v4tov4
```

显示所有的 portproxy，用

```powershell
netsh int portproxy reset all
```

重置（移除）所有 portproxy。
