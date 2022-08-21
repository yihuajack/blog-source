---
title: Pull request creation failed. Validation failed: you can‘t perform that action at this time.
categories: SuperUser
tags:
  - github
date: 2021-12-25 20:22:20
---

参考
Creating pull requests failed: HttpError: Validation Failed · Issue #1234 · microsoft/vscode-pull-request-github
https://github.com/microsoft/vscode-pull-request-github/issues/1234

Getting "Validation Failed" error message when trying to create PR · Issue #453 · github/VisualStudio
https://github.com/github/VisualStudio/issues/453
这是由于当前分支已被关联到一个 Pull Request，如果要新开一个 Pull Request 需要新建分支。
