---
title: ssh-add出现Error connecting to agent：No such file or directory的解决方法
categories: SuperUser
tags: ssh
abbrlink: 63648
date: 2020-09-28 14:31:37
---

Windows环境下执行

```
 ssh-add ~/.ssh/id_rsa
```

报错：

```
Error connecting to agent: No such file or directory
```

解决方法：【**以管理员身份运行**】执行

```
Set-Service ssh-agent -StartupType Manual
Start-Service ssh-agent
```