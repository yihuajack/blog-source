---
title: go get使用goproxy.cn仍然报错connectex: A connection attempt failed的解决方法
categories: SuperUser
tags:
  - go
  - goproxy
  - proxy
  - golang
  - gopls
date: 2021-02-13 12:42:03
---

执行

```powershell
go get -v golang.org/x/tools/gopls
```

报错：

> package golang.org/x/tools/gopls: unrecognized import path "golang.org/x/tools/gopls": https fetch: Get "https://golang.org/x/tools/gopls?go-get=1": dial tcp 216.239.37.1:443: connectex: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond.

经过一番查找所有的方法都称只要执行

```powershell
go env -w GOPROXY=https://goproxy.cn
```

或加上direct执行

```powershell
go env -w GOPROXY=https://goproxy.cn,direct
```

或执行

```powershell
go env -w GOSUMDB=off
```

把GOSUMDB关掉或设置Git的HTTP和HTTPS的代理的方法或设置为阿里云aliyun源，全部无效。最终在Goproxy China首页找到真正的解决方法：执行

```powershell
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

即可解决 。

注：可通过执行

```powershell
go env
```

命令查看所有的go环境变量。

再次执行

```powershell
go get -v golang.org/x/tools/gopls
```

输出：

> go: downloading golang.org/x/tools v0.1.0
> go: downloading golang.org/x/tools/gopls v0.6.5
> go: golang.org/x/tools/gopls upgrade => v0.6.5
> go: downloading golang.org/x/tools v0.1.1-0.20210201201750-4d4ee958a9b7
> go: downloading mvdan.cc/xurls/v2 v2.2.0
> go: downloading mvdan.cc/gofumpt v0.1.0
> go: downloading golang.org/x/mod v0.4.0
> go: downloading golang.org/x/xerrors v0.0.0-20200804184101-5ec99f83aff1
> go: downloading honnef.co/go/tools v0.0.1-2020.1.6
> go: downloading golang.org/x/sys v0.0.0-20210119212857-b64e53b001e4
> go: downloading golang.org/x/sync v0.0.0-20201020160332-67f06af15bc9
> go: downloading github.com/google/go-cmp v0.5.4
> go: downloading github.com/sergi/go-diff v1.1.0
> go: downloading github.com/BurntSushi/toml v0.3.1
> golang.org/x/mod/semver
> golang.org/x/xerrors/internal
> honnef.co/go/tools/arg
> honnef.co/go/tools/deprecated
> github.com/google/go-cmp/cmp/internal/flags
> golang.org/x/tools/internal/xcontext
> golang.org/x/tools/go/ast/astutil
> github.com/sergi/go-diff/diffmatchpatch
> golang.org/x/tools/internal/lsp/fuzzy
> golang.org/x/tools/internal/span
> golang.org/x/mod/internal/lazyregexp
> golang.org/x/xerrors
> golang.org/x/tools/go/analysis/passes/internal/analysisutil
> golang.org/x/tools/internal/analysisinternal
> golang.org/x/tools/go/ast/inspector
> golang.org/x/tools/go/types/typeutil
> golang.org/x/tools/internal/lsp/diff
> golang.org/x/tools/go/cfg
> golang.org/x/mod/module
> golang.org/x/tools/internal/event/label
> golang.org/x/tools/go/analysis
> golang.org/x/sys/execabs
> golang.org/x/tools/internal/fastwalk
> golang.org/x/tools/internal/event/keys
> golang.org/x/mod/modfile
> golang.org/x/tools/internal/lsp/diff/myers
> golang.org/x/tools/go/analysis/passes/asmdecl
> golang.org/x/tools/go/analysis/passes/inspect
> golang.org/x/tools/go/analysis/passes/buildtag
> golang.org/x/tools/go/analysis/passes/cgocall
> golang.org/x/tools/go/analysis/passes/assign
> golang.org/x/tools/go/analysis/passes/atomic
> golang.org/x/tools/go/analysis/passes/atomicalign
> golang.org/x/tools/go/analysis/passes/bools
> golang.org/x/tools/go/analysis/passes/composite
> golang.org/x/tools/go/analysis/passes/copylock
> golang.org/x/tools/go/analysis/passes/deepequalerrors
> golang.org/x/tools/go/analysis/passes/errorsas
> golang.org/x/tools/go/analysis/passes/fieldalignment
> golang.org/x/tools/go/analysis/passes/httpresponse
> golang.org/x/tools/go/analysis/passes/ifaceassert
> golang.org/x/tools/go/analysis/passes/loopclosure
> golang.org/x/tools/go/analysis/passes/ctrlflow
> golang.org/x/tools/go/analysis/passes/nilfunc
> golang.org/x/tools/go/analysis/passes/printf
> golang.org/x/tools/go/analysis/passes/shadow
> golang.org/x/tools/go/analysis/passes/shift
> golang.org/x/tools/go/analysis/passes/sortslice
> golang.org/x/tools/go/analysis/passes/stdmethods
> golang.org/x/tools/go/analysis/passes/stringintconv
> golang.org/x/tools/go/analysis/passes/lostcancel
> golang.org/x/tools/go/analysis/passes/structtag
> golang.org/x/tools/go/analysis/passes/testinggoroutine
> golang.org/x/tools/go/analysis/passes/tests
> golang.org/x/tools/go/analysis/passes/unmarshal
> golang.org/x/tools/go/analysis/passes/unreachable
> golang.org/x/tools/go/analysis/passes/unsafeptr
> golang.org/x/tools/go/analysis/passes/unusedresult
> golang.org/x/tools/internal/event/core
> golang.org/x/tools/internal/gopathwalk
> golang.org/x/tools/internal/lsp/analysis/fillreturns
> golang.org/x/tools/internal/lsp/analysis/fillstruct
> golang.org/x/tools/internal/lsp/analysis/nonewvars
> golang.org/x/tools/internal/lsp/analysis/noresultvalues
> golang.org/x/tools/internal/event
> golang.org/x/tools/internal/gocommand
> golang.org/x/tools/internal/lsp/analysis/simplifycompositelit
> golang.org/x/tools/internal/lsp/analysis/simplifyslice
> golang.org/x/tools/internal/lsp/analysis/undeclaredname
> golang.org/x/tools/internal/lsp/analysis/simplifyrange
> golang.org/x/tools/internal/lsp/analysis/unusedparams
> golang.org/x/tools/internal/lsp/debug/tag
> golang.org/x/tools/internal/imports
> golang.org/x/tools/internal/event/export
> golang.org/x/tools/refactor/satisfy
> honnef.co/go/tools/go/types/typeutil
> honnef.co/go/tools/ir
> golang.org/x/tools/internal/jsonrpc2
> golang.org/x/tools/go/internal/gcimporter
> golang.org/x/tools/go/internal/packagesdriver
> golang.org/x/tools/internal/packagesinternal
> golang.org/x/tools/internal/typesinternal
> golang.org/x/tools/go/types/objectpath
> golang.org/x/tools/internal/lsp/protocol
> golang.org/x/tools/go/gcexportdata
> honnef.co/go/tools/functions
> github.com/BurntSushi/toml
> honnef.co/go/tools/internal/passes/buildir
> golang.org/x/tools/go/packages
> honnef.co/go/tools/internal/robustio
> honnef.co/go/tools/facts
> golang.org/x/tools/go/buildutil
> golang.org/x/tools/internal/lsp/source
> honnef.co/go/tools/version
> honnef.co/go/tools/internal/renameio
> honnef.co/go/tools/loader
> honnef.co/go/tools/config
> golang.org/x/tools/go/internal/cgo
> honnef.co/go/tools/internal/cache
> honnef.co/go/tools/printf
> golang.org/x/tools/go/loader
> github.com/google/go-cmp/cmp/internal/diff
> github.com/google/go-cmp/cmp/internal/function
> github.com/google/go-cmp/cmp/internal/value
> mvdan.cc/xurls/v2
> golang.org/x/tools/internal/fakenet
> honnef.co/go/tools/ir/irutil
> golang.org/x/sync/errgroup
> github.com/google/go-cmp/cmp
> honnef.co/go/tools/lint
> golang.org/x/tools/internal/lsp/debug/log
> golang.org/x/tools/internal/memoize
> golang.org/x/tools/internal/event/export/metric
> golang.org/x/tools/internal/event/export/ocagent/wire
> golang.org/x/tools/internal/lsp/mod
> golang.org/x/tools/internal/lsp/snippet
> golang.org/x/tools/internal/lsp/cache
> golang.org/x/tools/internal/event/export/ocagent
> golang.org/x/tools/internal/event/export/prometheus
> honnef.co/go/tools/code
> honnef.co/go/tools/pattern
> honnef.co/go/tools/lint/lintutil/format
> honnef.co/go/tools/report
> mvdan.cc/gofumpt/format
> golang.org/x/tools/internal/lsp/source/completion
> honnef.co/go/tools/edit
> honnef.co/go/tools/lint/lintdsl
> golang.org/x/tools/internal/lsp/browser
> honnef.co/go/tools/lint/lintutil
> golang.org/x/tools/internal/tool
> golang.org/x/tools/internal/lsp/debug
> honnef.co/go/tools/internal/sharedcheck
> honnef.co/go/tools/stylecheck
> golang.org/x/tools/internal/lsp
> honnef.co/go/tools/simple
> honnef.co/go/tools/staticcheck
> golang.org/x/tools/internal/lsp/lsprpc
> golang.org/x/tools/internal/lsp/cmd
> golang.org/x/tools/gopls/internal/hooks
> golang.org/x/tools/gopls

成功。