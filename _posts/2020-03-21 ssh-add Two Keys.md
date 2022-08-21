---
title: ssh-add -L出现两个ssh-rsa key的解决办法
categories: CSDN补档
tags:
  - git
  - github
  - ssh
abbrlink: 31774
date: 2020-03-21 19:12:46
---

Windows上配置Git的困难之处在于往往不知道谁动了自己的ssh key：Git for Windows、msys2/mingw-w64中的git、CMD或PowerShell中的Git、WSL中的Git、其他软件中自带的Git等，比如最近GitHub将分支推送到Origin时突然报错，然后经过一番波折发现在Windows CMD命令提示符或PowerShell中输入ssh-add -l或ssh-add -L居然显示了两个不同的ssh-rsa key，而且指向的目录都是C:\Users\<username>\.ssh\id_rsa，重启计算机仍然如此，用ssh-keygen重新生成了key仍然如此，不得不说是一个神奇的bug。事实上，到最后也没能找出原因。据说GitHub Desktop与github.com和其他站点通信使用两套不同的SSH密钥，然而理论上它使用的密钥也会显示在$USERPROFILE/.ssh中，所以排除。不过经过测试，将其中的id_rsa删除并重启CMD后运行ssh-add -L命令仍然出现两个key，原来这条命令将SSH key存入SSH-Agent的高速缓存中，所以使用语句：

```
ssh-add -D
```

删除所有密钥（如果报错则先运行ssh-add -d直到不能删除然后再运行ssh-add D，不用担心，这条语句并不会删除id_rsa文件），然后重新运行

```
ssh-add C:\Users\<username>/.ssh/id_rsa
```

再次检验ssh-add -L只剩一个密钥了。重新打开GitHub Desktop，问题成功解决。

不过遗留的问题是如何根本上解决Windows SSH冲突的问题，比如Git for Windows中的SSH和WSL中的SSH，留待日后讨论。

------

2020年3月27日更新：

原因找到了，当我们使用JetBrains提供的CLion的WSL配置脚本后，它安装了一个SSH Server，并在WSL的~/.ssh/known_hosts末尾空一行添加了一行不同于普通ssh-rsa的代码，经过未知的操作，这段代码也在C:\Users\<username>\.ssh\known_hosts中出现。