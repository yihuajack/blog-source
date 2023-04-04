---
title: Git clone fetch-pack unexpected disconnect while reading sideband packet
categories: SuperUser
tags:
  - git
  - github
date: 2022-11-01 01:52:58
---

在执行 `git clone` 命令遇到以下错误：
```text
remote: Enumerating objects: 1252, done.
remote: Counting objects: 100% (1252/1252), done.
remote: Compressing objects: 100% (788/788), done.
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```
参考 [Github - unexpected disconnect while reading sideband packet](https://stackoverflow.com/questions/66366582/github-unexpected-disconnect-while-reading-sideband-packet)，对于 CMD，执行
```bat
set GIT_TRACE_PACKET=1
set GIT_TRACE=1
set GIT_CURL_VERBOSE=1
```
对于 Linux，执行
```bash
export GIT_TRACE_PACKET=1
export GIT_TRACE=1
export GIT_CURL_VERBOSE=1
```
对于 PowerShell，执行
```powershell
$env:GIT_TRACE_PACKET=1
$env:GIT_TRACE=1
$env:GIT_CURL_VERBOSE=1
```
然后执行
```bat
git config --global core.compression 0
git clone --depth 1 <repo_URI>
# cd to your newly created directory
git fetch --unshallow 
git pull --all
```
注意：这里的仓库 URI 必须为 HTTP（https://github.com/），不能为 SSH（git@github.com:）。