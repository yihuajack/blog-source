---
title: 更改 SSH 使用的 Shell
categories: SuperUser
tags:
  - ssh
  - 运维
  - wsl
  - openssh
date: 2021-10-29 13:28:25
---

本文参考
Choosing the shell that SSH uses? - Server Fault
https://serverfault.com/questions/106722/choosing-the-shell-that-ssh-uses

以 Windows 为例，其默认的 Shell 是 CMD，如果要使用 Windows 系统自带的 PowerShell 5 的话，则

```powershell
ssh -t user@host "powershell"
```

如果要使用新版的 PowerShell Core 的话，则

```powershell
ssh -t user@host "pwsh"
```

如果要使用 Git Bash 或 WSL 的话，根据你的 Windows 系统设置 bash 为哪个，使用

```powershell
ssh -t user@host "bash"
```

即可连接 Git Bash 或 WSL。

如果要在 Windows 中更改 SSH 连接的默认 Shell 的话，参考
OpenSSH Server Configuration for Windows | Microsoft Docs
https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_server_configuration

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

即可设置任意 Shell 为 OpenSSH for Windows 使用的默认 Shell。

如果使用的是 Linux 的话，则使用

```bash
ssh -t user@host 'zsh -l'
```

-t 表示的是强制使用伪TTY分配，-l 表示的是触发登录 Shell。此外，你还可以在你的 ~/.ssh/config 中修改 Host：

```
Host yourServer
    HostName <IP, FQDN or DNS resolvable name>
    IdentityFile ~/.ssh/<your keyfile>
    RemoteCommand zsh -l
    RequestTTY force
    User <yourUsername>
```

```
Host someHost
    HostName someIP
    IdentityFile ~/.ssh/somekey.pem
    RemoteCommand zsh -l -c 'sleep 1; source /tmp/somefile; zsh'
    PermitLocalCommand yes
    LocalCommand bash -c 'sftp %r@%h <<< "put /tmp/somefile /tmp/somefile"'
    RequestTTY force
    User someUser
```

