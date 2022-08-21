---
title: Windows OpenSSH Server 使用方法
categories: SuperUser
tags:
  - ssh
  - openssh
date: 2021-10-29 13:04:45
---

参考：

Install OpenSSH | Microsoft Docs
https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse
在 PowerShell Core 中检查 OpenSSH.Client 与 OpenSSH.Server 的安装状态：

```powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

如果未安装，使用

```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```
安装，输出：

> Path          :
> Online        : True
> RestartNeeded : False

执行

```powershell
# Start the sshd service
Start-Service sshd

# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'

# Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```
注意：不同于 Linux，我们无需更改 C:\ProgramData\ssh\sshd_config，使用默认端口 22 并防火墙放行即可。 上述命令检查是否已经创建了入站规则，但是可能不会显示在 Windows Defender 防火墙->高级设置->入站规则中，但是你可以在 Windows Defender 防火墙->监视->防火墙中找到这条入站规则。如果上述命令无效，参考 [ssh - Windows 10 - OpenSSH Server - Operation timed out - Stack Overflow](https://stackoverflow.com/questions/61509645/windows-10-openssh-server-operation-timed-out)执行

```powershell
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

通过控制面板打开 Windows Defender 防火墙->高级设置->入站规则，会发现名称：OpenSSH SSH Server (sshd)、组：OpenSSH Server、协议：TCP、端口：22 的规则已被创建并启用，或执行

```powershell
Get-NetFirewallRule -Name *ssh*
```

输出

> Name                          : OpenSSH-Server-In-TCP
> DisplayName                   : OpenSSH SSH Server (sshd)
> Description                   : Inbound rule for OpenSSH SSH Server (sshd)
> DisplayGroup                  : OpenSSH Server
> Group                         : OpenSSH Server
> Enabled                       : True
> Profile                       : Any
> Platform                      : {}
> Direction                     : Inbound
> Action                        : Allow
> EdgeTraversalPolicy           : Block
> LooseSourceMapping            : False
> LocalOnlyMapping              : False
> Owner                         :
> PrimaryStatus                 : OK
> Status                        : 已从存储区成功分析规则。 (65536)
> EnforcementStatus             : NotApplicable
> PolicyStoreSource             : PersistentStore
> PolicyStoreSourceType         : Local
> RemoteDynamicKeywordAddresses : {}
>
> Name                          : sshd
> DisplayName                   : OpenSSH Server (sshd)
> Description                   :
> DisplayGroup                  :
> Group                         :
> Enabled                       : True
> Profile                       : Any
> Platform                      : {}
> Direction                     : Inbound
> Action                        : Allow
> EdgeTraversalPolicy           : Block
> LooseSourceMapping            : False
> LocalOnlyMapping              : False
> Owner                         :
> PrimaryStatus                 : OK
> Status                        : 已从存储区成功分析规则。 (65536)
> EnforcementStatus             : NotApplicable
> PolicyStoreSource             : PersistentStore
> PolicyStoreSourceType         : Local
> RemoteDynamicKeywordAddresses : {}

运行->services.msc 打开服务，找到名称为 OpenSSH SSH Server 的服务（也有可能是 sshd），检查其是否已启动并且启动类型为自动。设置完成后，使用 ipconfig 命令找到本机的 IP 地址，例如 Wireless LAN adapter WLAN 的 IPv4 Address。在远程服务器中执行

```bash
ssh username@servername
```

其中 servername 即为本机的 IP 地址，username 为你的 Windows 用户名，然后输入的密码是你的 Windows 用户对应的密码。如果第一次连接会提示

> The authenticity of host 'servername (10.00.00.001)' can't be established. ECDSA key fingerprint is SHA256:(<a large string>). Are you sure you want to continue connecting (yes/no)?

输入 yes 即可成功连接。如果连接不成功，可以使用

```bash
netstat -ant | grep 22
```

检查端口 22 的状态，并使用

```bash
ssh localhost
```

和

```bash
ping <ip_address>
```

检查是否是 Windows 的 OpenSSH Server 的问题。

卸载 OpenSSH.Client 和 OpenSSH.Server 的方法是
```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```