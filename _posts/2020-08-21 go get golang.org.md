---
title: go get golang.org包失败的解决方法（Windows Terminal设置proxy）
categories: CSDN补档
tags:
  - go
  - proxy
abbrlink: 33918
date: 2020-08-21 21:25:20
---

Windows在编译安装YCM时在下载安装Omnisharp结束后报错：

```
Get "https://proxy.golang.org/golang.org/x/tools/gopls/@v/v0.4.2.info": dial tcp 34.64.4.17:443: connectex: A connection attempt failed because the connected party did
```

而在WSL Ubuntu中则没有这个问题，其原因是WSL Ubuntu已在zshrc文件中设置了proxy，而Windows中的proxy默认并不会给Terminal，所以需要手动配置。Linux中使用命令：

```bash
export http_proxy=http://proxyAddress:port
export https_proxy=http://proxyAddress:port
```

CMD中使用命令：

```bash
set http_proxy=http://proxyAddress:port
set https_proxy=http://proxyAddress:port
```

而在PowerShell中则使用命令：

```bash
$env:http_proxy=http://proxyAddress:port
$env:https_proxy=http://proxyAddress:port
```

由于部分ladder不通过ss，所以也不能本地proxy，故

```
netsh winhttp set proxy proxyaddress:port
```

无效。