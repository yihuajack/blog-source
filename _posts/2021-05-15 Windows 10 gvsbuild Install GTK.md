---
title: Windows 10使用gvsbuild安装配置GTK
categories: SuperUser
tags:
  - gtk/gtk+
  - gvsbuild
  - gvs
  - gtk
date: 2021-05-15 14:04:10
---

根据GTK官方网站[Setting up GTK for Windows](https://www.gtk.org/docs/installations/windows/)，在Windows上安装GTK有两种方法，一种是使用MSYS，这种方法较为简单，在此不表；另一种是使用gvsbuild，较为复杂，流程如下：

1. 安装dependencies，可在[gvsbuild](https://github.com/wingtk/gvsbuild)查看，当前版本为：Visual Studio 2019（2015及以上版本）、MSYS2 64、Python 3.9.2（3.6及以上版本）。更新MSYS2的包。

2. 将该库克隆到C:\gtk-build\github\gvsbuild，注意该路径不可更改，因为生成脚本规定如此。

3. 在PowerShell（CMD也可）中切换到该目录下并运行生成脚本：
   ```powershell
   cd C:\gtk-build\github\gvsbuild
   python .\build.py build -p x64 --vs-ver 16 gtk3 --msys-dir='E:\Program_Files\msys64' --vs-install-path='D:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise'
   ```

   其中-p -x64指定生成64位版本（默认为32位），--vs-ver 16指定VS生成工具版本为2019（2017版本号为15，2015版本号为14，2013版本号为12，默认为12）。注意脚本默认MSYS路径为C:\Msys64，所以需要手动指定你自己的MSYS安装路径；VS路径默认为C:\Program Files (x86)\Microsoft Visual Studio\\20xx，所以需要手动指定你自己的VS安装路径。详情可见源代码[gvsbuild/parser.py at master · wingtk/gvsbuild (github.com)](https://github.com/wingtk/gvsbuild/blob/master/gvsbuild/utils/parser.py)。脚本执行信息如下：

> Cleaning up the build environment
> Checking msys tool
> Checking Msvc tool
> Downloading packages
> C:\gtk-build\src\nuget-5.4.0.exe - Download finished
> C:\gtk-build\src\ninja-win-1.8.2.zip - Download finished
> C:\gtk-build\src\meson-0.57.1.tar.gz - Download finished
> C:\gtk-build\src\cmake-3.20.0-windows-x86_64.zip - Download finished
> C:\gtk-build\src\win-iconv-0.0.8.tar.gz - Download finished
> C:\gtk-build\src\gettext-0.19.7.tar.gz - Download finished
> C:\gtk-build\src\zlib-1.2.11.tar.xz - Download finished
> C:\gtk-build\src\glib-2.68.1.tar.xz - Download finished
> C:\gtk-build\src\atk-2.36.0.tar.xz - Download finished
> C:\gtk-build\src\nasm-2.13.03-win64.zip - Download finished
> C:\gtk-build\src\libjpeg-turbo-2.0.6.tar.gz - Download finished
> C:\gtk-build\src\tiff-4.2.0.tar.gz - Download finished
> C:\gtk-build\src\jasper-2.0.14.tar.gz - Download finished
> C:\gtk-build\src\libpng-1.6.37.tar.xz - Download finished
> C:\gtk-build\src\gdk-pixbuf-2.42.6.tar.xz - Download finished
> Opening http://git.savannah.gnu.org/cgit/freetype/freetype2.git/snapshot/freetype2-0d5f1dd37c056b4460a460d16fd1fbb06740e
> C:\gtk-build\src\freetype2-0d5f1dd37c056b4460a460d16fd1fbb06740e891.tar.gz - Download finished
> C:\gtk-build\src\libxml2-2.9.10.tar.gz - Download finished
> C:\gtk-build\src\fontconfig-2.13.0.tar.gz - Download finished
> C:\gtk-build\src\pixman-0.40.0.tar.gz - Download finished
> C:\gtk-build\src\cairo-1.16.0.tar.xz - Download finished
> C:\gtk-build\src\harfbuzz-2.8.0.tar.xz - Download finished
> C:\gtk-build\src\pango-1.48.4.tar.xz - Download finished
> C:\gtk-build\src\libepoxy-1.5.5.tar.xz - Download finished
> C:\gtk-build\src\gtk+-3.24.28.tar.xz - Download finished
> Building project nuget (5.4.0)
> Feeds used:
>   C:\Users\Yihua\.nuget\packages\
>   https://api.nuget.org/v3/index.json
>   C:\Users\Yihua\AppData\Local\Esri\NuGet
>   C:\Program Files (x86)\Microsoft SDKs\NuGetPackages\
>
> Attempting to gather dependency information for package 'python.3.9.2' with respect to project 'C:\gtk-build\tools', targeting 'Any,Version=v0.0'
> Gathering dependency information took 1.72 sec
> Attempting to resolve dependencies for package 'python.3.9.2' with DependencyBehavior 'Lowest'
> Resolving dependency information took 0 ms
> Resolving actions to install package 'python.3.9.2'
> Resolved actions to install package 'python.3.9.2'
> Retrieving package 'python 3.9.2' from 'nuget.org'.
>   GET https://api.nuget.org/v3-flatcontainer/python/3.9.2/python.3.9.2.nupkg
>   OK https://api.nuget.org/v3-flatcontainer/python/3.9.2/python.3.9.2.nupkg 835ms
> Installing python 3.9.2.
> Adding package 'python.3.9.2' to folder 'C:\gtk-build\tools'
> Added package 'python.3.9.2' to folder 'C:\gtk-build\tools'
> Successfully installed 'python 3.9.2' to C:\gtk-build\tools
> Executing nuget actions took 9.26 sec
> Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
> Collecting pip
>   Downloading https://pypi.tuna.tsinghua.edu.cn/packages/cd/6f/43037c7bcc8bd8ba7c9074256b1a11596daa15555808ec748048c1507f08/pip-21.1.1-py3-none-any.whl (1.5 MB)
>      |████████████████████████████████| 1.5 MB 939 kB/s
> Installing collected packages: pip
>   Attempting uninstall: pip
>     Found existing installation: pip 20.2.3
>     Uninstalling pip-20.2.3:
>       Successfully uninstalled pip-20.2.3
> Successfully installed pip-21.1.1
> Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
> Requirement already satisfied: setuptools in c:\gtk-build\tools\python.3.9.2\tools\lib\site-packages (49.2.1)
> ……

注意如果你为git设置了代理，中间python pip安装时代理会报错，需要关代理重新build，然后又会执行git clone操作因无代理而失败，需要开代理重新build，然后就开始编译了。注意：当前版本的wingtk有个bug，会导致编译错误

> Traceback (most recent call last):
>   File "C:\gtk-build\github\gvsbuild\gvsbuild\utils\builder.py", line 492, in build
>     if self.\__build_one(p):
>   File "C:\gtk-build\github\gvsbuild\gvsbuild\utils\builder.py", line 618, in __build_one
>     skip_deps = proj.build()
>   File "C:\gtk-build\github\gvsbuild\gvsbuild\projects.py", line 347, in build
>     content = f.read()
> UnicodeDecodeError: 'gbk' codec can't decode byte 0xbf in position 2: illegal multibyte sequence
> Error: fontconfig build failed

详情可见[Issue #411](https://github.com/wingtk/gvsbuild/issues/411)及[Pull Request #408](https://github.com/wingtk/gvsbuild/pull/408)，目前该PR尚未merge，可以在本地根据该PR修改projects.py后即可编译。
