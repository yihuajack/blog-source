---
title: git clone大仓库(>1G)时速度慢并出现RPC failed断开连接错误的真正解决方法
categories: CSDN补档
tags:
  - git clone
  - rpc failed
  - github
abbrlink: 24163
date: 2019-12-22 02:17:21
---

最近git clone一个很大的repository（git-sdk-64）时用它本身release的installer是解压出来一个小git然后用它来从远端clone但是速度极慢，并且出现错误

克隆远程存储库时遇到错误: Git failed with a fatal error.
early EOF
the remote end hung up unexpectedly
index-pack failed
RPC failed; curl 18 transfer closed with outstanding read data remaining

直接download zip用Microsoft Edge当然会断开，但是发现用Google Chrome和Internet Explorer也会断开，又试了Github Desktop也出问题，用安装了Github Extension的Visual Studio会速度慢点坚持时间长点不过仍然会报出相同的速度。考虑到网上的经验，先将github仓库用git云后台导入到gitee然后再从gitee下载zip发现同样会出现连接中断的问题，并且由于gitee下载速度较快（几个M/s）所以可以充分下载到略超过1G左右断开（其实使用offcloud下载Edge引出的zip下载链接尽管能提速但仍会在1G处断开），原来这是HTTP下载的问题(curl)，我们使用SSH clone即可（生成rsa pub那一套），注意gitee仓库和github一样对1G以上的仓库有限制，gitee会将导入的大仓库block掉，我们用掉一次解除的机会（一共3次）然后git clone ssh链接，注意到0.99/1.99/2.99处都会出现一条error，这并不影响，待其下载完resolve deltas然后update好后发现clone到的仓库完好。

经过试错证明，开全局模式代理、修改hosts文件并不能加快git clone的速度（导入到gitee才是正道再clone，同时期盼coding.net能从github导入），而设置git clone的深度为一 --depth 1也并不能解决克隆大仓库时断开的问题，也不要试图用各种downgit来分成一部分一部分的下载（因为下载很多后速度会极慢乃至于会出现服务器错误）。