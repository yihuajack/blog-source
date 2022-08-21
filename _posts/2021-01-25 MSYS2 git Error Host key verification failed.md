---
title: MSYS2 git报错Host key verification failed的解决方法
categories: SuperUser
tags:
  - msys
  - git
  - ssh
  - github
date: 2021-01-25 18:50:10
---

根据以往经验，这类问题往往是由HOME路径问题导致未找到正确的.ssh文件夹导致的。不同于Git for Windows的方法[HOME更改情况下Git for Windows报错git@github.com: Permission denied (publickey)的原因及解决方法](https://blog.csdn.net/yihuajack/article/details/108887898)，对于MSYS2参考[MinGW MSYS ssh error: Could not create directory '/home/<username>/.ssh'](https://superuser.com/questions/759407/mingw-msys-ssh-error-could-not-create-directory-home-username-ssh)，在MSYS2安装目录下的/etc/nsswitch.conf中修改db_home字段将

```
db_home: cygwin desc
```

改为

```
db_home: windows cygwin desc
```

重启MSYS2，执行

```bash
ssh -T git@github.com
```

成功。