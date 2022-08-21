---
title: npm或yarn安装sqlite3/express报错node-pre-gyp ERR! build/stack/configure error或卡住使用python+msvs编译
categories: SuperUser
tags:
date: 2021-01-18 11:23:35
---

执行类似

```powershell
yarn add sqlite3 --dev
```

或

```powershell
npm install -g sqlite3
```

的安装sqlite3或express的命令报错：

> ……yarn add v1.22.5
> warning package.json: No license field
> warning No license field
> [1/4] Resolving packages...
> warning sqlite3 > node-gyp > request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
> warning sqlite3 > node-gyp > request > har-validator@5.1.5: this library is no longer supported
> [2/4] Fetching packages...
> [3/4] Linking dependencies...
> [4/4] Building fresh packages...
> [1/2] ⡀ husky
> error D:\……\node_modules\sqlite3: Command failed.Exit code: 1
> Command: node-pre-gyp install --fallback-to-build
> Arguments:
> Directory: D:\……\node_modules\sqlite3
> Output:
> node-pre-gyp info it worked if it ends with ok
> node-pre-gyp info using node-pre-gyp@0.11.0
> node-pre-gyp info using node@14.15.4 | win32 | x64
> node-pre-gyp WARN Using request for node-pre-gyp https download
> node-pre-gyp info check checked for "D:\……\node_modules\sqlite3\lib\binding\napi-v6-win32-x64\node_sqlite3.node" (not found)
> node-pre-gyp http GET https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v5.0.1/napi-v6-win32-x64.tar.gz
> node-pre-gyp http 403 https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v5.0.1/napi-v6-win32-x64.tar.gz
> node-pre-gyp WARN Tried to download(403): https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v5.0.1/napi-v6-win32-x64.tar.gz
> node-pre-gyp WARN Pre-built binaries not found for sqlite3@5.0.1 and node@14.15.4 (node-v83 ABI, unknown) (falling back to source compile with node-gyp)
> node-pre-gyp http 403 status code downloading tarball https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v5.0.1/napi-v6-win32-x64.tar.gz
> gyp info it worked if it ends with ok
> gyp info using node-gyp@3.8.0
> gyp info using node@14.15.4 | win32 | x64
> gyp info ok
> gyp info it worked if it ends with ok
> gyp info using node-gyp@3.8.0
> gyp info using node@14.15.4 | win32 | x64
> gyp ERR! configure error
> gyp ERR! stack Error: Command failed: E:\Program_Files\Python39\python3.EXE -c import sys; print "%s.%s.%s" % sys.version_info[:3];
> gyp ERR! stack   File "<string>", line 1
> gyp ERR! stack     import sys; print "%s.%s.%s" % sys.version_info[:3];
> gyp ERR! stack                       ^
> gyp ERR! stack SyntaxError: invalid syntax
> gyp ERR! stack
> gyp ERR! stack     at ChildProcess.exithandler (child_process.js:308:12)
> gyp ERR! stack     at ChildProcess.emit (events.js:315:20)
> gyp ERR! stack     at maybeClose (internal/child_process.js:1048:16)
> gyp ERR! stack     at Process.ChildProcess.\_handle.onexit (internal/child_process.js:288:5)
> gyp ERR! System Windows_NT 10.0.19042_
>
> gyp ERR! command "E:\\Program_Files\\nodejs\\node.exe" "D:\\……\\node_modules\\node-gyp\\bin\\node-gyp.js" "configure" "--fallback-to-build" "--module=D:\\……\\node_modules\\sqlite3\\lib\\binding\\napi-v6-win32-x64\\node_sqlite3.node" "--module_name=node_sqlite3" "--module_path=D:\\……\\node_modules\\sqlite3\\lib\\binding\\napi-v6-win32-x64" "--napi_version=7" "--node_abi_napi=napi" "--napi_build_version=6" "--node_napi_label=napi-v6" "--python=python3"
> gyp ERR! cwd D:\……\node_modules\sqlite3
> gyp ERR! node -v v14.15.4
> gyp ERR! node-gyp -v v3.8.0
> gyp ERR! not ok
> node-pre-gyp ERR! build error
> node-pre-gyp ERR! stack Error: Failed to execute 'E:\Program_Files\nodejs\node.exe D:\……\node_modules\node-gyp\bin\node-gyp.js configure --fallback-to-build --module=D:\……\node_modules\sqlite3\lib\binding\napi-v6-win32-x64\node_sqlite3.node --module_name=node_sqlite3 --module_path=D:\……\node_modules\sqlite3\lib\binding\napi-v6-win32-x64 --napi_version=7 --node_abi_napi=napi --napi_build_version=6 --node_napi_label=napi-v6 --python=python3' (1)
> node-pre-gyp ERR! stack     at ChildProcess.<anonymous> (D:\……\node_modules\node-pre-gyp\lib\util\compile.js:83:29)
> node-pre-gyp ERR! stack     at ChildProcess.emit (events.js:315:20)
> node-pre-gyp ERR! stack     at maybeClose (internal/child_process.js:1048:16)
> node-pre-gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:288:5)
> node-pre-gyp ERR! System Windows_NT 10.0.19042
> node-pre-gyp ERR! command "E:\\Program_Files\\nodejs\\node.exe" "D:\\……\\node_modules\\node-pre-gyp\\bin\\node-pre-gyp" "install" "--fallback-to-build"
> node-pre-gyp ERR! cwd D:\……\node_modules\sqlite3
> node-pre-gyp ERR! node -v v14.15.4
> node-pre-gyp ERR! node-pre-gyp -v v0.11.0
> node-pre-gyp ERR! not ok
> Failed to execute 'E:\Program_Files\nodejs\node.exe D:\……\node_modules\node-gyp\bin\node-gyp.js configure --fallback-to-build --module=D:\……\node_modules\sqlite3\lib\binding\napi-v6-win32-x64\node_sqlite3.node --module_name=node_sqlite3 --module_path=D:\……\node_modules\sqlite3\lib\binding\napi-v6-win32-x64 --napi_version=7 --node_abi_napi=napi --napi_build_version=6 --node_napi_label=napi-v6 --python=python3' (1)

 等信息。其原因在于当前最新版本的sqlite3 5.0.1并没有发布预编译二进制包，只发布了源码，所以npm/yarn会自己编译源码生成package，但是自己编译就会出现各种各样的问题，以下是解决方法：

```powershell
npm install --global --dev --verbose windows-build-tools # optional
npm config set python python3 # optional
npm config set msvs_version 2019 --global # depends on msvs version on your machine
```

如果有条件请使用代理+registry=http://registry.npmjs.org。执行

```powershell
npm install --global --also=dev --verbose sqlite3
```

或

```powershell
npm install -save-dev --verbose sqlite3
```

显示如下信息：

> npm info it worked if it ends with ok
> npm verb cli [
> npm verb cli   'E:\\Program_Files\\nodejs\\node.exe',
> npm verb cli   'E:\\Program_Files\\nodejs\\node_modules\\npm\\bin\\npm-cli.js',
> npm verb cli   'install',
> npm verb cli   '--global',
> npm verb cli   '--also=dev',
> npm verb cli   '--verbose',
> npm verb cli   'sqlite3'
> npm verb cli ]
> npm info using npm@6.14.10
> npm info using node@v14.15.4
> npm verb npm-session 18497903379a94e0
> npm http fetch GET 304 http://registry.npmjs.org/sqlite3 445ms (from cache)
> npm timing stage:loadCurrentTree Completed in 484ms
> npm timing stage:loadIdealTree:cloneCurrentTree Completed in 0ms
> npm timing stage:loadIdealTree:loadShrinkwrap Completed in 3ms
> npm http fetch GET 304 http://registry.npmjs.org/node-gyp 395ms (from cache)
> ……
> npm http fetch GET 304 http://registry.npmjs.org/minizlib 425ms (from cache)
> npm timing stage:loadIdealTree:loadAllDepsIntoIdealTree Completed in 15295ms
> npm timing stage:loadIdealTree Completed in 15336ms
> npm timing stage:generateActionsToTake Completed in 28ms
> npm verb correctMkdir E:\Program_Files\npm-cache\_locks correctMkdir not in flight; initializing
> npm verb makeCacheDir UID & GID are irrelevant on win32
> npm verb lock using E:\Program_Files\npm-cache\_locks\staging-7477ffcb210b7af1.lock for E:\Program_Files\npm\node_modules\.staging
> npm timing action:extract Completed in 513ms
> npm timing action:finalize Completed in 699ms
> npm timing action:refresh-package-json Completed in 418ms
> npm info lifecycle abbrev@1.1.1~preinstall: abbrev@1.1.1
> ……
> npm info lifecycle debug@3.2.7~preinstall: debug@3.2.7
> npm timing action:preinstall Completed in 38ms
> npm info linkStuff abbrev@1.1.1
> ……
> npm info linkStuff sqlite3@5.0.1
> npm timing action:build Completed in 106ms
> npm info lifecycle abbrev@1.1.1~install: abbrev@1.1.1
> ……
> npm info lifecycle sqlite3@5.0.1~install: sqlite3@5.0.1

> sqlite3@5.0.1 install E:\Program_Files\npm\node_modules\sqlite3
> node-pre-gyp install --fallback-to-build
>
> node-pre-gyp info it worked if it ends with ok
> node-pre-gyp verb cli [
> node-pre-gyp verb cli   'E:\\Program_Files\\nodejs\\node.exe',
> node-pre-gyp verb cli   'E:\\Program_Files\\npm\\node_modules\\sqlite3\\node_modules\\node-pre-gyp\\bin\\node-pre-gyp',
> node-pre-gyp verb cli   'install',
> node-pre-gyp verb cli   '--fallback-to-build'
> node-pre-gyp verb cli ]
> node-pre-gyp info using node-pre-gyp@0.11.0
> node-pre-gyp info using node@14.15.4 | win32 | x64
> node-pre-gyp verb command install [ 'napi_build_version=6' ]
> node-pre-gyp WARN Using request for node-pre-gyp https download
> node-pre-gyp info check checked for "E:\Program_Files\npm\node_modules\sqlite3\lib\binding\napi-v6-win32-x64\node_sqlite3.node" (not found)
> node-pre-gyp http GET https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v5.0.1/napi-v6-win32-x64.tar.gz
> node-pre-gyp verb download using proxy url: "http://127.0.0.1:11000/"
> node-pre-gyp http 403 https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v5.0.1/napi-v6-win32-x64.tar.gz
> node-pre-gyp WARN Tried to download(403): https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v5.0.1/napi-v6-win32-x64.tar.gz
> node-pre-gyp WARN Pre-built binaries not found for sqlite3@5.0.1 and node@14.15.4 (node-v83 ABI, unknown) (falling back to source compile with node-gyp)
> node-pre-gyp http 403 status code downloading tarball https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v5.0.1/napi-v6-win32-x64.tar.gz
> node-pre-gyp verb command build [ 'rebuild', 'napi_build_version=6' ]
> gyp info it worked if it ends with ok
> gyp verb cli [
> gyp verb cli   'E:\\Program_Files\\nodejs\\node.exe',
> gyp verb cli   'E:\\Program_Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gyp.js',
> gyp verb cli   'clean'
> gyp verb cli ]
> gyp info using node-gyp@5.1.0
> gyp info using node@14.15.4 | win32 | x64
> gyp verb command clean []
> gyp verb clean removing "build" directory
> gyp info ok
> gyp info it worked if it ends with ok
> gyp verb cli [
> gyp verb cli   'E:\\Program_Files\\nodejs\\node.exe',
> gyp verb cli   'E:\\Program_Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gyp.js',
> gyp verb cli   'configure',
> gyp verb cli   '--fallback-to-build',
> gyp verb cli   '--module=E:\\Program_Files\\npm\\node_modules\\sqlite3\\lib\\binding\\napi-v6-win32-x64\\node_sqlite3.node',
> gyp verb cli   '--module_name=node_sqlite3',
> gyp verb cli   '--module_path=E:\\Program_Files\\npm\\node_modules\\sqlite3\\lib\\binding\\napi-v6-win32-x64',
> gyp verb cli   '--napi_version=7',
> gyp verb cli   '--node_abi_napi=napi',
> gyp verb cli   '--napi_build_version=6',
> gyp verb cli   '--node_napi_label=napi-v6',
> gyp verb cli   '--python=python3'
> gyp verb cli ]
> gyp info using node-gyp@5.1.0
> gyp info using node@14.15.4 | win32 | x64
> gyp verb command configure []
> gyp verb find Python checking Python explicitly set from command line or npm configuration
> gyp verb find Python - "--python=" or "npm config get python" is "python3"
> gyp verb find Python - executing "python3" to get executable path
> gyp verb find Python - executable path is "E:\Program_Files\Python39\python3.exe"
> gyp verb find Python - executing "E:\Program_Files\Python39\python3.exe" to get version
> gyp verb find Python - version is "3.9.1"
> gyp info find Python using Python version 3.9.1 found at "E:\Program_Files\Python39\python3.exe"
> gyp verb get node dir no --target version specified, falling back to host node version: 14.15.4
> gyp verb command install [ '14.15.4' ]
> gyp verb install input version string "14.15.4"
> gyp verb install installing version: 14.15.4
> gyp verb install --ensure was passed, so won't reinstall if already installed
> gyp verb install version is already installed, need to check "installVersion"
> gyp verb got "installVersion" 9
> gyp verb needs "installVersion" 9
> gyp verb install version is good
> gyp verb get node dir target node version installed: 14.15.4
> gyp verb build dir attempting to create "build" dir: E:\Program_Files\npm\node_modules\sqlite3\build
> gyp verb build dir "build" dir needed to be created? E:\Program_Files\npm\node_modules\sqlite3\build
> gyp verb find VS msvs_version not set from command line or npm config
> gyp verb find VS VCINSTALLDIR not set, not running in VS Command Prompt
> gyp verb find VS checking VS2019 (16.8.30907.101) found at:
> gyp verb find VS "D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise"
> gyp verb find VS - found "Visual Studio C++ core features"
> gyp verb find VS - found VC++ toolset: v142
> gyp verb find VS - found Windows SDK: 10.0.19041.0
> gyp info find VS using VS2019 (16.8.30907.101) found at:
> gyp info find VS "D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise"
> gyp info find VS run with --verbose for detailed information
> gyp verb build/config.gypi creating config file
> gyp verb build/config.gypi writing out config file: E:\Program_Files\npm\node_modules\sqlite3\build\config.gypi
> gyp verb config.gypi checking for gypi file: E:\Program_Files\npm\node_modules\sqlite3\config.gypi
> gyp verb common.gypi checking for gypi file: E:\Program_Files\npm\node_modules\sqlite3\common.gypi
> gyp verb gyp gyp format was not specified; forcing "msvs"
> gyp info spawn E:\Program_Files\Python39\python3.exe
> gyp info spawn args [
> gyp info spawn args   'E:\\Program_Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\gyp\\gyp_main.py',
> gyp info spawn args   'binding.gyp',
> gyp info spawn args   '-f',
> gyp info spawn args   'msvs',
> gyp info spawn args   '-I',
> gyp info spawn args   'E:\\Program_Files\\npm\\node_modules\\sqlite3\\build\\config.gypi',
> gyp info spawn args   '-I',
> gyp info spawn args   'E:\\Program_Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\addon.gypi',
> gyp info spawn args   '-I',
> gyp info spawn args   'C:\\Users\\Yihua\\AppData\\Local\\node-gyp\\Cache\\14.15.4\\include\\node\\common.gypi',
> gyp info spawn args   '-Dlibrary=shared_library',
> gyp info spawn args   '-Dvisibility=default',
> gyp info spawn args   '-Dnode_root_dir=C:\\Users\\Yihua\\AppData\\Local\\node-gyp\\Cache\\14.15.4',
> gyp info spawn args   '-Dnode_gyp_dir=E:\\Program_Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp',
> gyp info spawn args   '-Dnode_lib_file=C:\\\\Users\\\\Yihua\\\\AppData\\\\Local\\\\node-gyp\\\\Cache\\\\14.15.4\\\\<(target_arch)\\\\node.lib',
> gyp info spawn args   '-Dmodule_root_dir=E:\\Program_Files\\npm\\node_modules\\sqlite3',
> gyp info spawn args   '-Dnode_engine=v8',
> gyp info spawn args   '--depth=.',
> gyp info spawn args   '--no-parallel',
> gyp info spawn args   '--generator-output',
> gyp info spawn args   'E:\\Program_Files\\npm\\node_modules\\sqlite3\\build',
> gyp info spawn args   '-Goutput_dir=.'
> gyp info spawn args ]
> gyp info ok
> gyp info it worked if it ends with ok
> gyp verb cli [
> gyp verb cli   'E:\\Program_Files\\nodejs\\node.exe',
> gyp verb cli   'E:\\Program_Files\\nodejs\\node_modules\\npm\\node_modules\\node-gyp\\bin\\node-gyp.js',
> gyp verb cli   'build',
> gyp verb cli   '--fallback-to-build',
> gyp verb cli   '--module=E:\\Program_Files\\npm\\node_modules\\sqlite3\\lib\\binding\\napi-v6-win32-x64\\node_sqlite3.node',
> gyp verb cli   '--module_name=node_sqlite3',
> gyp verb cli   '--module_path=E:\\Program_Files\\npm\\node_modules\\sqlite3\\lib\\binding\\napi-v6-win32-x64',
> gyp verb cli   '--napi_version=7',
> gyp verb cli   '--node_abi_napi=napi',
> gyp verb cli   '--napi_build_version=6',
> gyp verb cli   '--node_napi_label=napi-v6'
> gyp verb cli ]
> gyp info using node-gyp@5.1.0
> gyp info using node@14.15.4 | win32 | x64
> gyp verb command build []
> gyp verb build type Release
> gyp verb architecture x64
> gyp verb node dev dir C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4
> gyp verb found first Solution file build/binding.sln
> gyp verb using MSBuild: D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\MSBuild\Current\Bin\MSBuild.exe
> gyp info spawn D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\MSBuild\Current\Bin\MSBuild.exe
> gyp info spawn args [
> gyp info spawn args   'build/binding.sln',
> gyp info spawn args   '/nologo',
> gyp info spawn args   '/p:Configuration=Release;Platform=x64'
> gyp info spawn args ]
> 在此解决方案中一次生成一个项目。若要启用并行生成，请添加“-m”开关。
> 生成启动时间为 2021/1/18 10:57:08。
> 节点 1 上的项目“E:\Program_Files\npm\node_modules\sqlite3\build\binding.sln”(默认目标)。
> ValidateSolutionConfiguration:
>   正在生成解决方案配置“Release|x64”。
> 项目“E:\Program_Files\npm\node_modules\sqlite3\build\binding.sln”(1)正在节点 1 上生成“E:\Program_Files\npm\node_modules\sqlite3\
> build\action_after_build.vcxproj.metaproj”(2) (默认目标)。
> 项目“E:\Program_Files\npm\node_modules\sqlite3\build\action_after_build.vcxproj.metaproj”(2)正在节点 1 上生成“E:\Program_Files\n
> pm\node_modules\sqlite3\build\node_sqlite3.vcxproj.metaproj”(3) (默认目标)。
> 项目“E:\Program_Files\npm\node_modules\sqlite3\build\node_sqlite3.vcxproj.metaproj”(3)正在节点 1 上生成“E:\Program_Files\npm\nod
> e_modules\sqlite3\build\node_modules\node-addon-api\nothing.vcxproj”(4) (默认目标)。
> PrepareForBuild:
>   正在创建目录“Release\obj\nothing\”。
>   正在创建目录“E:\Program_Files\npm\node_modules\sqlite3\build\Release\”。
>   正在创建目录“Release\obj\nothing\nothing.tlog\”。
> InitializeBuildStatus:
>   正在创建“Release\obj\nothing\nothing.tlog\unsuccessfulbuild”，因为已指定“AlwaysCreate”。
> ClCompile:
>   D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Tools\MSVC\14.28.29333\bin\HostX64\x64\CL.exe /c /I
>   "C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\include\node" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.
>   15.4\src" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\openssl\config" /I"C:\Users\Yihua\AppData\Local
>   \node-gyp\Cache\14.15.4\deps\openssl\openssl\include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\uv\
>   include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\zlib" /I"C:\Users\Yihua\AppData\Local\node-gyp\C
>   ache\14.15.4\deps\v8\include" /Z7 /nologo /W3 /WX- /diagnostics:column /MP /Ox /Ob2 /Oi /Ot /Oy /D NODE_GYP_MODULE_NA
>   ME=nothing /D USING_UV_SHARED=1 /D USING_V8_SHARED=1 /D V8_DEPRECATION_WARNINGS=1 /D V8_DEPRECATION_WARNINGS /D V8_IM
>   MINENT_DEPRECATION_WARNINGS /D WIN32 /D _CRT_SECURE_NO_DEPRECATE /D _CRT_NONSTDC_NO_DEPRECATE /D _HAS_EXCEPTIONS=0 /D
>    OPENSSL_NO_PINSHARED /D OPENSSL_THREADS /D "HOST_BINARY=\"node.exe\"" /GF /Gm- /MT /GS /Gy /fp:precise /Zc:wchar_t /
>   Zc:forScope /Zc:inline /GR- /Fo"Release\obj\nothing\\" /Fd"Release\obj\nothing\nothing.pdb" /Gd /TC /wd4351 /wd4355 /
>   wd4800 /wd4251 /wd4275 /wd4244 /wd4267 /FC /errorReport:queue "..\..\..\node_modules\node-addon-api\nothing.c"
>   nothing.c
>   D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Tools\MSVC\14.28.29333\bin\HostX64\x64\CL.exe /c /I
>   "C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\include\node" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.
>   15.4\src" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\openssl\config" /I"C:\Users\Yihua\AppData\Local
>   \node-gyp\Cache\14.15.4\deps\openssl\openssl\include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\uv\
>   include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\zlib" /I"C:\Users\Yihua\AppData\Local\node-gyp\C
>   ache\14.15.4\deps\v8\include" /Z7 /nologo /W3 /WX- /diagnostics:column /MP /Ox /Ob2 /Oi /Ot /Oy /D NODE_GYP_MODULE_NA
>   ME=nothing /D USING_UV_SHARED=1 /D USING_V8_SHARED=1 /D V8_DEPRECATION_WARNINGS=1 /D V8_DEPRECATION_WARNINGS /D V8_IM
>   MINENT_DEPRECATION_WARNINGS /D WIN32 /D _CRT_SECURE_NO_DEPRECATE /D _CRT_NONSTDC_NO_DEPRECATE /D _HAS_EXCEPTIONS=0 /D
>    OPENSSL_NO_PINSHARED /D OPENSSL_THREADS /D "HOST_BINARY=\"node.exe\"" /GF /Gm- /MT /GS /Gy /fp:precise /Zc:wchar_t /
>   Zc:forScope /Zc:inline /GR- /Fo"Release\obj\nothing\\" /Fd"Release\obj\nothing\nothing.pdb" /Gd /TP /wd4351 /wd4355 /
>   wd4800 /wd4251 /wd4275 /wd4244 /wd4267 /FC /errorReport:queue "E:\Program_Files\nodejs\node_modules\npm\node_modules\
>   node-gyp\src\win_delay_load_hook.cc"
>   win_delay_load_hook.cc
> Lib:
>   D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Tools\MSVC\14.28.29333\bin\HostX64\x64\Lib.exe /OUT
>   :"E:\Program_Files\npm\node_modules\sqlite3\build\Release\nothing.lib" /NOLOGO /MACHINE:X64 Release\obj\nothing\nothi
>   ng.obj
>   Release\obj\nothing\win_delay_load_hook.obj
>   nothing.vcxproj -> E:\Program_Files\npm\node_modules\sqlite3\build\Release\\nothing.lib
> FinalizeBuildStatus:
>   正在删除文件“Release\obj\nothing\nothing.tlog\unsuccessfulbuild”。
>   正在对“Release\obj\nothing\nothing.tlog\nothing.lastbuildstate”执行 Touch 任务。
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\node_modules\node-addon-api\nothing.vcxproj”(默认目标)的 操作。
>
> 项目“E:\Program_Files\npm\node_modules\sqlite3\build\node_sqlite3.vcxproj.metaproj”(3)正在节点 1 上生成“E:\Program_Files\npm\nod
> e_modules\sqlite3\build\deps\action_before_build.vcxproj”(5) (默认目标)。
> PrepareForBuild:
>   正在创建目录“Release\obj\action_before_build\”。
>   正在创建目录“Release\obj\action_before_build\action_b.B7F51064.tlog\”。
> InitializeBuildStatus:
>   正在创建“Release\obj\action_before_build\action_b.B7F51064.tlog\unsuccessfulbuild”，因为已指定“AlwaysCreate”。
> ComputeCustomBuildOutput:
>   正在创建目录“E:\Program_Files\npm\node_modules\sqlite3\build\Release\obj\global_intermediate\sqlite-autoconf-3320300\”。
> CustomBuild:
>   unpack_sqlite_dep
> FinalizeBuildStatus:
>   正在删除文件“Release\obj\action_before_build\action_b.B7F51064.tlog\unsuccessfulbuild”。
>   正在对“Release\obj\action_before_build\action_b.B7F51064.tlog\action_before_build.lastbuildstate”执行 Touch 任务。
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\deps\action_before_build.vcxproj”(默认目标)的操作。
>
> 项目“E:\Program_Files\npm\node_modules\sqlite3\build\node_sqlite3.vcxproj.metaproj”(3)正在节点 1 上生成“E:\Program_Files\npm\nod
> e_modules\sqlite3\build\deps\sqlite3.vcxproj.metaproj”(6) (默认目标)。
> 项目“E:\Program_Files\npm\node_modules\sqlite3\build\deps\sqlite3.vcxproj.metaproj”(6)正在节点 1 上生成“E:\Program_Files\npm\nod
> e_modules\sqlite3\build\deps\sqlite3.vcxproj”(7) (默认目标)。
> PrepareForBuild:
>   正在创建目录“Release\obj\sqlite3\”。
>   正在创建目录“Release\obj\sqlite3\sqlite3.tlog\”。
> InitializeBuildStatus:
>   正在创建“Release\obj\sqlite3\sqlite3.tlog\unsuccessfulbuild”，因为已指定“AlwaysCreate”。
> ClCompile:
>   D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Tools\MSVC\14.28.29333\bin\HostX64\x64\CL.exe /c /I
>   "C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\include\node" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.
>   15.4\src" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\openssl\config" /I"C:\Users\Yihua\AppData\Local
>   \node-gyp\Cache\14.15.4\deps\openssl\openssl\include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\uv\
>   include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\zlib" /I"C:\Users\Yihua\AppData\Local\node-gyp\C
>   ache\14.15.4\deps\v8\include" /I"E:\Program_Files\npm\node_modules\sqlite3\build\Release\obj\global_intermediate\sqli
>   te-autoconf-3320300" /Z7 /nologo /W3 /WX- /diagnostics:column /MP /Ox /Ob2 /Oi /Ot /Oy /D NODE_GYP_MODULE_NAME=sqlite
>   3 /D USING_UV_SHARED=1 /D USING_V8_SHARED=1 /D V8_DEPRECATION_WARNINGS=1 /D V8_DEPRECATION_WARNINGS /D V8_IMMINENT_DE
>   PRECATION_WARNINGS /D WIN32 /D \_CRT_SECURE_NO_DEPRECATE /D _CRT_NONSTDC_NO_DEPRECATE /D _HAS_EXCEPTIONS=0 /D OPENSSL_
>   NO_PINSHARED /D OPENSSL_THREADS /D \_REENTRANT=1 /D SQLITE_THREADSAFE=1 /D HAVE_USLEEP=1 /D SQLITE_ENABLE_FTS3 /D SQLI
>   TE_ENABLE_FTS4 /D SQLITE_ENABLE_FTS5 /D SQLITE_ENABLE_JSON1 /D SQLITE_ENABLE_RTREE /D SQLITE_ENABLE_DBSTAT_VTAB=1 /D
>   "HOST_BINARY=\"node.exe\"" /D NDEBUG /GF /Gm- /EHsc /MT /GS /Gy /fp:precise /Zc:wchar_t /Zc:forScope /Zc:inline /GR-
>   /Fo"Release\obj\sqlite3\\" /Fd"Release\obj\sqlite3\sqlite3.pdb" /Gd /TC /wd4351 /wd4355 /wd4800 /wd4251 /wd4275 /wd42
>   44 /wd4267 /FC /errorReport:queue "E:\Program_Files\npm\node_modules\sqlite3\build\Release\obj\global_intermediate\sq
>   lite-autoconf-3320300\sqlite3.c"
>   sqlite3.c
>   D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Tools\MSVC\14.28.29333\bin\HostX64\x64\CL.exe /c /I
>   "C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\include\node" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.
>   15.4\src" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\openssl\config" /I"C:\Users\Yihua\AppData\Local
>   \node-gyp\Cache\14.15.4\deps\openssl\openssl\include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\uv\
>   include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\zlib" /I"C:\Users\Yihua\AppData\Local\node-gyp\C
>   ache\14.15.4\deps\v8\include" /I"E:\Program_Files\npm\node_modules\sqlite3\build\Release\obj\global_intermediate\sqli
>   te-autoconf-3320300" /Z7 /nologo /W3 /WX- /diagnostics:column /MP /Ox /Ob2 /Oi /Ot /Oy /D NODE_GYP_MODULE_NAME=sqlite
>   3 /D USING_UV_SHARED=1 /D USING_V8_SHARED=1 /D V8_DEPRECATION_WARNINGS=1 /D V8_DEPRECATION_WARNINGS /D V8_IMMINENT_DE
>   PRECATION_WARNINGS /D WIN32 /D \_CRT_SECURE_NO_DEPRECATE /D \_CRT_NONSTDC_NO_DEPRECATE /D \_HAS_EXCEPTIONS=0 /D OPENSSL_
>   NO_PINSHARED /D OPENSSL_THREADS /D \_REENTRANT=1 /D SQLITE_THREADSAFE=1 /D HAVE_USLEEP=1 /D SQLITE_ENABLE_FTS3 /D SQLI
>   TE_ENABLE_FTS4 /D SQLITE_ENABLE_FTS5 /D SQLITE_ENABLE_JSON1 /D SQLITE_ENABLE_RTREE /D SQLITE_ENABLE_DBSTAT_VTAB=1 /D
>   "HOST_BINARY=\"node.exe\"" /D NDEBUG /GF /Gm- /EHsc /MT /GS /Gy /fp:precise /Zc:wchar_t /Zc:forScope /Zc:inline /GR-
>   /Fo"Release\obj\sqlite3\\" /Fd"Release\obj\sqlite3\sqlite3.pdb" /Gd /TP /wd4351 /wd4355 /wd4800 /wd4251 /wd4275 /wd42
>   44 /wd4267 /FC /errorReport:queue "E:\Program_Files\nodejs\node_modules\npm\node_modules\node-gyp\src\win_delay_load_
>   hook.cc"
>   win_delay_load_hook.cc
> Lib:
>   D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Tools\MSVC\14.28.29333\bin\HostX64\x64\Lib.exe /OUT
>   :"E:\Program_Files\npm\node_modules\sqlite3\build\Release\sqlite3.lib" /NOLOGO /MACHINE:X64 Release\obj\sqlite3\sqlit
>   e3.obj
>   Release\obj\sqlite3\win_delay_load_hook.obj
>   sqlite3.vcxproj -> E:\Program_Files\npm\node_modules\sqlite3\build\Release\\sqlite3.lib
> FinalizeBuildStatus:
>   正在删除文件“Release\obj\sqlite3\sqlite3.tlog\unsuccessfulbuild”。
>   正在对“Release\obj\sqlite3\sqlite3.tlog\sqlite3.lastbuildstate”执行 Touch 任务。
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\deps\sqlite3.vcxproj”(默认目标)的操作。
>
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\deps\sqlite3.vcxproj.metaproj”(默认目标)的操作。
>
> 项目“E:\Program_Files\npm\node_modules\sqlite3\build\node_sqlite3.vcxproj.metaproj”(3)正在节点 1 上生成“E:\Program_Files\npm\nod
> e_modules\sqlite3\build\node_sqlite3.vcxproj”(8) (默认目标)。
> PrepareForBuild:
>   正在创建目录“Release\obj\node_sqlite3\”。
>   正在创建目录“Release\obj\node_sqlite3\node_sqlite3.tlog\”。
> InitializeBuildStatus:
>   正在创建“Release\obj\node_sqlite3\node_sqlite3.tlog\unsuccessfulbuild”，因为已指定“AlwaysCreate”。
> ClCompile:
>   D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Tools\MSVC\14.28.29333\bin\HostX64\x64\CL.exe /c /I
>   "C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\include\node" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.
>   15.4\src" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\openssl\config" /I"C:\Users\Yihua\AppData\Local
>   \node-gyp\Cache\14.15.4\deps\openssl\openssl\include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\uv\
>   include" /I"C:\Users\Yihua\AppData\Local\node-gyp\Cache\14.15.4\deps\zlib" /I"C:\Users\Yihua\AppData\Local\node-gyp\C
>   ache\14.15.4\deps\v8\include" /I"E:\Program_Files\npm\node_modules\sqlite3\node_modules\node-addon-api" /I"E:\Program
>   \_Files\npm\node_modules\sqlite3\build\Release\obj\global_intermediate\sqlite-autoconf-3320300" /Z7 /nologo /W3 /WX- /
>   diagnostics:column /MP /Ox /Ob2 /Oi /Ot /Oy /D NODE_GYP_MODULE_NAME=node_sqlite3 /D USING_UV_SHARED=1 /D USING_V8_SHA
>   RED=1 /D V8_DEPRECATION_WARNINGS=1 /D V8_DEPRECATION_WARNINGS /D V8_IMMINENT_DEPRECATION_WARNINGS /D WIN32 /D \_CRT_SE
>   CURE_NO_DEPRECATE /D \_CRT_NONSTDC_NO_DEPRECATE /D \_HAS_EXCEPTIONS=0 /D OPENSSL_NO_PINSHARED /D OPENSSL_THREADS /D NAP
>   I_VERSION=6 /D NAPI_DISABLE_CPP_EXCEPTIONS=1 /D SQLITE_THREADSAFE=1 /D HAVE_USLEEP=1 /D SQLITE_ENABLE_FTS3 /D SQLITE_
>   ENABLE_FTS4 /D SQLITE_ENABLE_FTS5 /D SQLITE_ENABLE_JSON1 /D SQLITE_ENABLE_RTREE /D SQLITE_ENABLE_DBSTAT_VTAB=1 /D BUI
>   LDING_NODE_EXTENSION /D "HOST_BINARY=\"node.exe\"" /D NDEBUG /D \_WINDLL /GF /Gm- /EHsc /MT /GS /Gy /fp:precise /Zc:wc
>   har_t /Zc:forScope /Zc:inline /GR- /Fo"Release\obj\node_sqlite3\\" /Fd"Release\obj\node_sqlite3\vc142.pdb" /Gd /TP /w
>   d4351 /wd4355 /wd4800 /wd4251 /wd4275 /wd4244 /wd4267 /FC /errorReport:queue ..\src\backup.cc ..\src\database.cc ..\s
>   rc\node_sqlite3.cc ..\src\statement.cc "E:\Program_Files\nodejs\node_modules\npm\node_modules\node-gyp\src\win_delay_
>   load_hook.cc"
>   backup.cc
>   database.cc
>   node_sqlite3.cc
>   statement.cc
>   win_delay_load_hook.cc
> Link:
>   D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Tools\MSVC\14.28.29333\bin\HostX64\x64\link.exe /ER
>   RORREPORT:QUEUE /OUT:"E:\Program_Files\npm\node_modules\sqlite3\build\Release\node_sqlite3.node" /NOLOGO kernel32.lib
>    user32.lib gdi32.lib winspool.lib comdlg32.lib advapi32.lib shell32.lib ole32.lib oleaut32.lib uuid.lib odbc32.lib D
>   elayImp.lib "C:\\Users\\Yihua\\AppData\\Local\\node-gyp\\Cache\\14.15.4\\x64\\node.lib" Delayimp.lib /DELAYLOAD:node.
>   exe /MANIFEST /MANIFESTUAC:"level='asInvoker' uiAccess='false'" /manifest:embed /DEBUG /PDB:"E:\Program_Files\npm\nod
>   e_modules\sqlite3\build\Release\node_sqlite3.pdb" /TLBID:1 /DYNAMICBASE /NXCOMPAT /MACHINE:X64 /ignore:4199 /DLL Rele
>   ase\obj\node_sqlite3\backup.obj
>   Release\obj\node_sqlite3\database.obj
>   Release\obj\node_sqlite3\node_sqlite3.obj
>   Release\obj\node_sqlite3\statement.obj
>   Release\obj\node_sqlite3\win_delay_load_hook.obj
>   E:\Program_Files\npm\node_modules\sqlite3\build\Release\nothing.lib
>   E:\Program_Files\npm\node_modules\sqlite3\build\Release\sqlite3.lib
>     正在创建库 E:\Program_Files\npm\node_modules\sqlite3\build\Release\node_sqlite3.lib 和对象 E:\Program_Files\npm\node_module
>   s\sqlite3\build\Release\node_sqlite3.exp
>   node_sqlite3.vcxproj -> E:\Program_Files\npm\node_modules\sqlite3\build\Release\\node_sqlite3.node
> FinalizeBuildStatus:
>   正在删除文件“Release\obj\node_sqlite3\node_sqlite3.tlog\unsuccessfulbuild”。
>   正在对“Release\obj\node_sqlite3\node_sqlite3.tlog\node_sqlite3.lastbuildstate”执行 Touch 任务。
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\node_sqlite3.vcxproj”(默认目标)的操作。
>
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\node_sqlite3.vcxproj.metaproj”(默认目标)的操作。
>
> 项目“E:\Program_Files\npm\node_modules\sqlite3\build\action_after_build.vcxproj.metaproj”(2)正在节点 1 上生成“E:\Program_Files\n
> pm\node_modules\sqlite3\build\action_after_build.vcxproj”(9) (默认目标)。
> PrepareForBuild:
>   正在创建目录“Release\obj\action_after_build\”。
>   正在创建目录“Release\obj\action_after_build\action_a.61BF2BE5.tlog\”。
> InitializeBuildStatus:
>   正在创建“Release\obj\action_after_build\action_a.61BF2BE5.tlog\unsuccessfulbuild”，因为已指定“AlwaysCreate”。
> CustomBuild:
>   Copying E:\Program_Files\npm\node_modules\sqlite3\build\Release\/node_sqlite3.node to E:/Program_Files/npm/node_modul
>   es/sqlite3/lib/binding/napi-v6-win32-x64\node_sqlite3.node
>   已复制         1 个文件。
> FinalizeBuildStatus:
>   正在删除文件“Release\obj\action_after_build\action_a.61BF2BE5.tlog\unsuccessfulbuild”。
>   正在对“Release\obj\action_after_build\action_a.61BF2BE5.tlog\action_after_build.lastbuildstate”执行 Touch 任务。
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\action_after_build.vcxproj”(默认目标)的操作。
>
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\action_after_build.vcxproj.metaproj”(默认目标)的操作。
>
> 已完成生成项目“E:\Program_Files\npm\node_modules\sqlite3\build\binding.sln”(默认目标)的操作。
>
> 已成功生成。
>     0 个警告
>     0 个错误
>
> 已用时间 00:00:17.30
> gyp info ok
> node-pre-gyp info ok
> npm verb lifecycle sqlite3@5.0.1~install: unsafe-perm in lifecycle true
> npm verb lifecycle sqlite3@5.0.1~install: PATH: ……
> npm verb lifecycle sqlite3@5.0.1~install: CWD: E:\Program_Files\npm\node_modules\sqlite3
> npm timing action:install Completed in 21161ms
> npm info lifecycle abbrev@1.1.1~postinstall: abbrev@1.1.1
> ……
> npm info lifecycle sqlite3@5.0.1~postinstall: sqlite3@5.0.1
> npm timing action:postinstall Completed in 49ms
> npm verb unlock done using E:\Program_Files\npm-cache\_locks\staging-7477ffcb210b7af1.lock for E:\Program_Files\npm\node_modules\.staging
> npm timing stage:executeActions Completed in 23022ms
> npm timing stage:rollbackFailedOptional Completed in 0ms
> npm timing stage:runTopLevelLifecycles Completed in 38880ms
>
> \+ sqlite3@5.0.1
> added 120 packages from 167 contributors in 38.882s
> npm verb exit [ 0, true ]
> npm timing npm Completed in 39298ms
> npm info ok

安装成功。express同理。