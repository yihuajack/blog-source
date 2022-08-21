---
title: ssh: connect to host github.com port 22: Connection timed out的一种原因
categories: SuperUser
tags:
  - ssh
  - github
  - dns服务器
date: 2020-10-25 09:49:48
---

先检查是否可以裸连github.com，如果不能则可能由于DNS解析的问题，需要等待DNS服务修复。这里更换DNS解析很难解决问题，使用代理虽然可以连接，但是SSH将仍处于不可用的状态。这种情况下参考[Solution for 'ssh: connect to host github.com port 22: Connection timed out' error](https://gist.github.com/Tamal/1cc77f88ef3e900aeae65f0e5e504794)、[【git 端口拒绝解决方案】ssh: connect to host github.com port 22: Connection refused](https://blog.csdn.net/s740556472/article/details/80318886)等方式更换端口为443或设置ProxyCommand等方法自然是无效的，等到网络修复好后自然可以顺利使用SSH方式连接。