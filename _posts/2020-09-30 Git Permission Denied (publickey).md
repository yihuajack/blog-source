---
title: 非一般情况下(HOME更改)git@github.com：Permission denied (publickey)的原因及解决方法
categories: SuperUser
tags:
  - git
  - ssh
  - openssh
abbrlink: 28957
date: 2020-09-30 21:48:11
---

在Windows CMD或PowerShell中执行git clone命令报错：

```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
 
Please make sure you have the correct access rights
and the repository exists.
```

该问题只出现在使用SSH方式克隆的情形下。

首先要根据GitHub官方文档[Error: Permission denied (publickey)](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/error-permission-denied-publickey)逐一检查排除。经检查，Windows环境中不存在sudo的问题，连接到github.com使用端口22，使用正确的

```
ssh -T git@github.com
```

并出现成功提示：

```
warning: agent returned different signature type ssh-rsa (expected rsa-sha2-512)
Hi yihuajack! You've successfully authenticated, but GitHub does not provide shell access.
```

确认ssh-agent已启动。执行

```
ssh-add -l
```

显示出2048 SHA256码及正确的id_rsa路径。参考[Calculate RSA key fingerprint](https://stackoverflow.com/questions/9607295/calculate-rsa-key-fingerprint)执行

```
ssh-keygen -lf /path/to/ssh/key
```

显示出2048 SHA256码及正确的配置邮箱，执行

```
ssh-keygen -E md5 -lf <fileName>
```

显示出2048 MD5码及正确的配置邮箱。将SHA256码和MD5码与GitHub->Settings->SSH and GPG keys->SSH keys对应的key比较发现完全一致。执行

```
ssh -vT git@github.com
```

（其中T可省略）显示

```
OpenSSH_for_Windows_7.7p1, LibreSSL 2.6.5
debug1: Reading configuration data C:\\Users\\Yihua/.ssh/config
debug1: Connecting to github.com [52.74.223.119] port 22.
debug1: Connection established.
debug1: identity file C:\\Users\\Yihua/.ssh/id_rsa type 0
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_rsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_dsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_dsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_ecdsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_ecdsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_ed25519 type -1
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_ed25519-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_xmss type -1
debug1: key_load_public: No such file or directory
debug1: identity file C:\\Users\\Yihua/.ssh/id_xmss-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_for_Windows_7.7
debug1: Remote protocol version 2.0, remote software version babeld-88a6481e
debug1: no match: babeld-88a6481e
debug1: Authenticating to github.com:22 as 'git'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: rsa-sha2-512
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
...
debug1: Host 'github.com' is known and matches the RSA host key.
debug1: Found key in C:\\Users\\Yihua/.ssh/known_hosts:1
debug1: rekey after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey after 134217728 blocks
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,ssh-rsa-cert-v01@openssh.com,ssh-dss-cert-v01@openssh.com,ssh-ed25519,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256,rsa-sha2-512,rsa-sha2-256,ssh-rsa,ssh-dss>
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
...
debug1: Server accepts key: pkalg ssh-rsa blen 279
warning: agent returned different signature type ssh-rsa (expected rsa-sha2-512)
debug1: Authentication succeeded (publickey).
Authenticated to github.com ([52.74.223.119]:22).
debug1: channel 0: new [client-session]
debug1: Entering interactive session.
debug1: pledge: network
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
Hi yihuajack! You've successfully authenticated, but GitHub does not provide shell access.
debug1: channel 0: free: client-session, nchannels 1
Transferred: sent 2560, received 2468 bytes, in 0.8 seconds
4
```

检查完没有错误后，在WSL Ubuntu中发现可以正常克隆，进一步发现在正确配置Git for Windows (Git Bash)、MSYS2 (MinGW-w64)、Cygwin64后在三个环境中也同样可以正常克隆。Git Bash中显示debug信息如下：

```
OpenSSH_8.3p1, OpenSSL 1.1.1g  21 Apr 2020
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: Connecting to github.com [13.229.188.59] port 22.
debug1: Connection established.
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_rsa type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_rsa-cert type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_dsa type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_dsa-cert type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_ecdsa type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_ecdsa-cert type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_ecdsa_sk type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_ecdsa_sk-cert type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_ed25519 type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_ed25519-cert type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_ed25519_sk type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_ed25519_sk-cert type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_xmss type -1
debug1: identity file /e/Cadence/SPB_Data/.ssh/id_xmss-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.3
debug1: Remote protocol version 2.0, remote software version babeld-88a6481e
debug1: no match: babeld-88a6481e
debug1: Authenticating to github.com:22 as 'git'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: rsa-sha2-512
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
...
debug1: Host 'github.com' is known and matches the RSA host key.
debug1: Found key in /e/Cadence/SPB_Data/.ssh/known_hosts:1
debug1: rekey out after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 134217728 blocks
debug1: Will attempt key: /c/Users/Yihua/.ssh/id_rsa RSA 
...
debug1: Will attempt key: /e/Cadence/SPB_Data/.ssh/id_rsa
debug1: Will attempt key: /e/Cadence/SPB_Data/.ssh/id_dsa
debug1: Will attempt key: /e/Cadence/SPB_Data/.ssh/id_ecdsa
debug1: Will attempt key: /e/Cadence/SPB_Data/.ssh/id_ecdsa_sk
debug1: Will attempt key: /e/Cadence/SPB_Data/.ssh/id_ed25519
debug1: Will attempt key: /e/Cadence/SPB_Data/.ssh/id_ed25519_sk
debug1: Will attempt key: /e/Cadence/SPB_Data/.ssh/id_xmss
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,ssh-rsa-cert-v01@openssh.com,ssh-dss-cert-v01@openssh.com,ssh-ed25519,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256,rsa-sha2-512,rsa-sha2-256,ssh-rsa,ssh-dss>
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering public key: /c/Users/Yihua/.ssh/id_rsa RSA 
...
debug1: Server accepts key: /c/Users/Yihua/.ssh/id_rsa RSA 
...
debug1: Authentication succeeded (publickey).
Authenticated to github.com ([13.229.188.59]:22).
debug1: channel 0: new [client-session]
debug1: Entering interactive session.
debug1: pledge: network
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
Hi yihuajack! You've successfully authenticated, but GitHub does not provide shell access.
debug1: channel 0: free: client-session, nchannels 1
Transferred: sent 2832, received 2468 bytes, in 0.7 seconds
Bytes per second: sent 4004.1, received 3489.4
debug1: Exit status 1
```

WSL Ubuntu显示信息如下：

```
OpenSSH_8.2p1 Ubuntu-4ubuntu0.1, OpenSSL 1.1.1f  31 Mar 2020
debug1: Reading configuration data /home/ayka_tsuzuki/.ssh/config
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: include /etc/ssh/ssh_config.d/*.conf matched no files
debug1: /etc/ssh/ssh_config line 21: Applying options for *
debug1: Connecting to github.com [13.250.177.223] port 22.
debug1: Connection established.
debug1: identity file /home/ayka_tsuzuki/.ssh/id_rsa type 0
debug1: identity file /home/ayka_tsuzuki/.ssh/id_rsa-cert type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_dsa type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_dsa-cert type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_ecdsa type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_ecdsa-cert type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_ecdsa_sk type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_ecdsa_sk-cert type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_ed25519 type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_ed25519-cert type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_ed25519_sk type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_ed25519_sk-cert type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_xmss type -1
debug1: identity file /home/ayka_tsuzuki/.ssh/id_xmss-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.1
debug1: Remote protocol version 2.0, remote software version babeld-88a6481e
debug1: no match: babeld-88a6481e
debug1: Authenticating to github.com:22 as 'git'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: rsa-sha2-512
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
...
debug1: Host 'github.com' is known and matches the RSA host key.
debug1: Found key in /home/ayka_tsuzuki/.ssh/known_hosts:1
debug1: rekey out after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 134217728 blocks
debug1: Will attempt key: /home/ayka_tsuzuki/.ssh/id_rsa RSA 
...
debug1: Will attempt key: /home/ayka_tsuzuki/.ssh/id_dsa
debug1: Will attempt key: /home/ayka_tsuzuki/.ssh/id_ecdsa
debug1: Will attempt key: /home/ayka_tsuzuki/.ssh/id_ecdsa_sk
debug1: Will attempt key: /home/ayka_tsuzuki/.ssh/id_ed25519
debug1: Will attempt key: /home/ayka_tsuzuki/.ssh/id_ed25519_sk
debug1: Will attempt key: /home/ayka_tsuzuki/.ssh/id_xmss
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,ssh-rsa-cert-v01@openssh.com,ssh-dss-cert-v01@openssh.com,ssh-ed25519,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256,rsa-sha2-512,rsa-sha2-256,ssh-rsa,ssh-dss>
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering public key: /home/ayka_tsuzuki/.ssh/id_rsa RSA 
...
debug1: Server accepts key: /home/ayka_tsuzuki/.ssh/id_rsa RSA 
...
debug1: Authentication succeeded (publickey).
Authenticated to github.com ([13.250.177.223]:22).
debug1: channel 0: new [client-session]
debug1: Entering interactive session.
debug1: pledge: network
debug1: Sending environment.
debug1: Sending env LANG = C.UTF-8
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
Hi yihuajack! You've successfully authenticated, but GitHub does not provide shell access.
debug1: channel 0: free: client-session, nchannels 1
Transferred: sent 2796, received 2468 bytes, in 0.7 seconds
Bytes per second: sent 3818.8, received 3370.8
debug1: Exit status 1
```

发现在git-cmd.exe（cmd）也就是Windows CMD/PowerShell使用的git中使用git clone验证RSA key fingerprint自动创建在HOME环境变量目录下也就是E:/Cadence/SPB_Data下.ssh/known_hosts，如果在git-bash.exe（bash）中配置export HOME=C:/Users/<username>则会自动完成ssh-add的工作。参考[安装适用于 Windows Server 2019 和 Windows 10 的 OpenSSH](https://docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse)使用PowerShell安装[Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH)后参考[Configure SSH Key and Git Integration WithWindows 10 Native Way](https://medium.com/rkttu/set-up-ssh-key-and-git-integration-in-windows-10-native-way-c9b94952dd2c)该问题仍然存在。由此可以确定这是由于HOME被设置成环境变量指向另一个文件夹所致，尽管执行echo $HOME命令会指向正确的个人文件夹，debug信息也均指向正确的个人文件夹。同时，即使允许其在HOME环境变量文件夹下新建.ssh/known_hosts，该问题仍然会出现。此外，使用setx命令或手动在控制面板中创建/修改HOME环境变量或删除HOME环境变量无济于事。

最终解决方案（参考[SSH is looking in the wrong place for the public/private key pair on Windows](https://stackoverflow.com/questions/2840871/ssh-is-looking-in-the-wrong-place-for-the-public-private-key-pair-on-windows)）：

将Git安装目录下的/etc/nsswitch.conf中的db_home一行由

```
db_home: env windows cygwin desc
```

修改为

```
db_home: /%H
```

成功解决。

后记：将Win32-OpenSSH解压到C:\Program Files\下后将其环境变量添加并上移到C:\WINDOWS\System32\OpenSSH\之前，再次执行ssh -v git@github.com，显示已使用新版的OpenSSH for Windows：

```
OpenSSH_for_Windows_8.1p1, LibreSSL 2.9.2
```