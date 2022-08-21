---
title: WSL(Ubuntu)pip安装genpac后找不到命令的解决方法
categories: CSDN补档
tags:
  - ubuntu
  - pip
abbrlink: 60680
date: 2020-03-29 15:22:49
---

在WSL Ubuntu使用pip成功安装genpac后运行genpac --version却显示

```
zsh: command not found: genpac
```

输入

```bash
pip list
```

后发现genpac已经在列表中。解决方法是在.zshrc（如果不用ZSH则是.bashrc）文件中添加：

```
export PATH=$HOME/bin:/usr/local/bin:$PATH
export PATH=$HOME/.local/bin:/usr/local/bin:$PATH
```

后genpac可以成功运行。执行ls ~/.local/bin得到：

```
genpac  pip  pip2  pip2.7
```

这个目录是pip的安装目录，对于pip安装的其他包也同样适用。