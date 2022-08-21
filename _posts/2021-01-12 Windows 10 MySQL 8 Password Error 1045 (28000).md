---
title: Windows 10 MySQL 8密码正确报错ERROR 1045 (28000): Access denied的原因
categories: SuperUser
tags:
  - mysql
date: 2021-01-12 16:23:59
---

在Windows 10下使用mysql-installer-community-8.0.22.0.msi安装MySQL，配置阶段为DB Admin用户设置密码配置不成功，但是root成功了，所以暂时没管，但在CMD/PowerShell中运行

```powershell
mysql -u root -p
```

或类似命令回车后输入正确的密码登录时却报错：

> ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)

 即使将密码写在-p后面也无济于事，由于密码本身输入是正确的，所以网上几乎所有解决这个问题的方式改密码（不管是5.7版本之前还是之后）都不会解决（甚至有文章会告诉你先将密码改成空的，这是不被允许的，如果你试图在Enter password:后直接回车会直接提示

> ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)

采用strong password encryption authentication method和legacy authentication method都报错。在各种无用信息中翻找许久之后找到一篇文章：[MySQL 1045 error Access Denied triggers in the following cases](https://www.percona.com/blog/2019/07/05/fixing-a-mysql-1045-error/)的第5条：Special characters in the password being converted by Bash，虽然根据这篇文章给在-p后面的密码加上单引号没有用，但是却启发了可能是密码本身的问题，原先的密码包括了一个反斜杠\，于是重装了MySQL Server（注意如果要取消安装务必要配置结束安装完成后再卸载，否则可能会出现难以预料的错误），在安装配置阶段设置了一个新密码，同时为DB Admin用户也设置了一个密码，符号只带下划线和横杠的，安装完成后执行前述命令：

> Enter password: 一堆星号
> Welcome to the MySQL monitor.  Commands end with ; or \g.
> Your MySQL connection id is 12
> Server version: 8.0.22 MySQL Community Server - GPL
>
> Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
>
> Oracle is a registered trademark of Oracle Corporation and/or its
> affiliates. Other names may be trademarks of their respective
> owners.
>
> Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
>
> mysql>

问题成功解决。