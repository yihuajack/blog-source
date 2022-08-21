---
title: Windows Terminal更改主题为iTerm2-Color-Schemes
categories: CSDN补档
abbrlink: 43478
date: 2020-03-25 11:39:00
tags:
---

在美化Windows Terminal的过程中，由于Windows Terminal处于preview阶段更新变化极大，导致走了一些弯路，现以0.10.781.0版本为例（到目前为止最新发布）。

注意：[Adding New Color Schemes to Windows Terminal](https://andrewpla.dev/Adding-New-Color-Schemes-To-Windows-Terminal/)中的脚本[Add-WindowsTerminalSchemes.ps1](https://gist.github.com/AndrewPla/5c302e91af5448c89a65bfab364249d8#file-add-windowsterminalschemes-ps1)经检验无法被Windows Terminal成功加载，该文章下的评论也显示了种种问题。一种可能的原因是其在profiles.json中错误地在最外层加入了一对方括号。如果已经执行了该脚本而不想重置，删除这些额外添加的方括号、“null"等并恢复原状。

从[iTerm2-Color-Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes) clone或下载到本地，进入windowsterminal目录下找到你喜欢的主题的json文件打开，然后将其全选（包括最外层大括号）粘贴到profiles.json里的"schemes"中，然后在"profiles"里的"lists"里的你想应用于的块中添加行"colorScheme": "<schemename>"，<schemename>为你在scheme中粘贴的主题的名字，如果想对每个shell都应用这个主题只需要在"profiles"下的"defaults"中添加这行即可。你也可以在"schemes"中粘贴多个主题。

可参考我的profiles.json：https://gist.github.com/yihuajack/eebf809607e6fdb79e035c4184173de8