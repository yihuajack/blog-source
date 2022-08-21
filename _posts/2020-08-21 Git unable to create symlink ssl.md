---
title: >-
  git错误unable to create symlink ssl...Could not reset index file to revision
  ‘HEAD‘的解决方法
categories: CSDN补档
tags: git
abbrlink: 57803
date: 2020-08-21 12:32:37
---

执行

```bash
git reset --hard
```

时报错：

```
error: unable to create symlink ssl: Permission denied
fatal: Could not reset index file to revision 'HEAD'
```

解决方法是以管理员身份运行Terminal或Shell。