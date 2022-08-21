---
title: Windows gVim和NeoVim绕过HOME更改_vimrc和vimfiles路径及配置SpaceVim的方法
categories: SuperUser
tags:
  - vim
  - gvim
date: 2020-12-28 17:40:42
---

受到[Change your home directory in vim](https://superuser.com/questions/307953/change-your-home-directory-in-vim)的启发在gVim中分别执行

```
:help VIMINIT
:help 'runtimepath'
```

查看帮助文档，分别在Vim安装目录下的vim82/doc文件夹下的starting.txt的第824行起和options.txt的第6227行起：

>                 \*'runtimepath'* \*'rtp'* \*vimfiles*
'runtimepath' 'rtp'    string    (default:
                    Unix: "\$HOME/.vim,
                        \$VIM/vimfiles,
                        \$VIMRUNTIME,
                        \$VIM/vimfiles/after,
                        \$HOME/.vim/after"
                    Amiga: "home:vimfiles,
                        \$VIM/vimfiles,
                        \$VIMRUNTIME,
                        \$VIM/vimfiles/after,
                        home:vimfiles/after"
                    PC:    "\$HOME/vimfiles,
                        \$VIM/vimfiles,
                        \$VIMRUNTIME,
                        \$VIM/vimfiles/after,
                        \$HOME/vimfiles/after"
                    macOS: "\$VIM:vimfiles,
                        \$VIMRUNTIME,
                        \$VIM:vimfiles:after"
                    Haiku: "\$BE_USER_SETTINGS/vim,
                        \$VIM/vimfiles,
                        \$VIMRUNTIME,
                        \$VIM/vimfiles/after,
                        \$BE_USER_SETTINGS/vim/after")
                    VMS: "sys\$login:vimfiles,
                        \$VIM/vimfiles,
                        \$VIMRUNTIME,
                        \$VIM/vimfiles/after,
                        sys$login:vimfiles/after")
            global
    This is a list of directories which will be searched for runtime

>        \*VIMINIT* \*.vimrc* \*_vimrc* \*EXINIT* \*.exrc* \*_exrc* \*\$MYVIMRC*
     c. Five places are searched for initializations.  The first that exists
    is used, the others are ignored.  The \$MYVIMRC environment variable is
    set to the file that was first found, unless \$MYVIMRC was already set
    and when using VIMINIT.
    I   The environment variable VIMINIT (see also |compatible-default|) (\*)
        The value of \$VIMINIT is used as an Ex command line.
    II  The user vimrc file(s):
            "\$HOME/.vimrc"       (for Unix) (\*)
            "\$HOME/.vim/vimrc"       (for Unix) (\*)
            "s:.vimrc"           (for Amiga) (\*)
            "home:.vimrc"       (for Amiga) (\*)
            "home:vimfiles:vimrc"  (for Amiga) (\*)
            "\$VIM/.vimrc"       (for Amiga) (\*)
            "\$HOME/\_vimrc"       (for Win32) (\*)
            "\$HOME/vimfiles/vimrc" (for Win32) (\*)
            "\$VIM/_vimrc"       (for Win32) (\*)
            "\$HOME/config/settings/vim/vimrc"    (for Haiku) (\*)_

  ​      Note: For Unix and Amiga, when ".vimrc" does not exist,
  ​      "\_vimrc" is also tried, in case an MS-DOS compatible file
  ​      system is used.  For MS-Windows ".vimrc" is checked after
  ​      "\_vimrc", in case long file names are used.
  ​      Note: For Win32, "\$HOME" is checked first.  If no "_vimrc" or
  ​      ".vimrc" is found there, "\$VIM" is tried.  See |\$VIM| for when
  ​      $VIM is not set.

 据此设置两个系统变量：

- VIM    值为要更改到的_vimrc和vimfiles的路径
- VIMRUNTIME    值为vim安装目录下的vim82文件夹

要使用SpaceVim，则将VIM设置为克隆的路径如D:\Documents\Programming\SpaceVim，重新打开gVim即生效。

注意：这种方法经全面验证，可能会出现意料之外的状况。

对于NeoVim：在NeoVim安装目录（使用scoop安装的目录在scoop安装目录下的apps\newovim\版本号下）下的share\nvim\runtime\doc\startings.txt的第440行起和

>                         \*VIMINIT* \*EXINIT* \*\$MYVIMRC*
     b. Four places are searched for initializations.  The first that exists
    is used, the others are ignored.  The \$MYVIMRC environment variable is
    set to the file that was first found, unless $MYVIMRC was already set
    and when using VIMINIT.
  
    -  Environment variable $VIMINIT, used as an Ex command line.
    -  User |config| file: $XDG_CONFIG_HOME/nvim/init.vim.
    -  Other config file: {xdg_config_dir}/nvim/init.vim where 
       {xdg_config_dir} is one of the directories in $XDG_CONFIG_DIRS.
    -  Environment variable $EXINIT, used as an Ex command line.

>                 \*'runtimepath'* \*'rtp'* \*vimfiles*
'runtimepath' 'rtp'    string    (default:     "\$XDG_CONFIG_HOME/nvim,
                           \$XDG_CONFIG_DIRS[1]/nvim,
                           \$XDG_CONFIG_DIRS[2]/nvim,
                           …
                           \$XDG_DATA_HOME/nvim[-data]/site,
                           \$XDG_DATA_DIRS[1]/nvim/site,
                           \$XDG_DATA_DIRS[2]/nvim/site,
                           …
                           \$VIMRUNTIME,
                           …
                           \$XDG_DATA_DIRS[2]/nvim/site/after,
                           \$XDG_DATA_DIRS[1]/nvim/site/after,
                           \$XDG_DATA_HOME/nvim[-data]/site/after,
                           …
                           \$XDG_CONFIG_DIRS[2]/nvim/after,
                           \$XDG_CONFIG_DIRS[1]/nvim/after,
                           $XDG_CONFIG_HOME/nvim/after")
            global

由于之前已经手动设置了XDG_CONFIG_HOME，所以init.vim和运行时文件都会在XDG_CONFIG_HOME下。若使用SpaceVim，使用软链接（符号链接symlink）为$XDG_CONFIG_HOME/nvim <<===>> SpaceVim文件夹创建符号链接，重启NeoVim即生效。

创建符号链接的方法为以管理员身份运行命令提示符，执行命令

```powershell
mklink "source_path" "destination_path"
```

注意到当环境变量VIMRUNTIME设置后尽管NeoVim仍旧会采用$XDG_CONFIG_HOME/nvim，但是会产生一些冲突，所以如果同时使用NeoVim不推荐使用更改环境变量的方式更改路径，而应通过分别创建符号链接的方式更改路径。

配置SpaceVim样例：（预设环境变量HOME值为E:\Cadence\SPB_Data，环境变量XDG_CONFIG_HOME值为D:\Documents\Programming\emacshome）执行

```powershell
cd D:\Documents\Programming\vimhome
git clone https://github.com/SpaceVim/SpaceVim.git .SpaceVim
```

克隆SpaceVim库。执行

```powershell
mklink "E:\Cadence\SPB_Data\vimfiles" "D:\Documents\Programming\vimhome\.SpaceVim"
```

为gVim创建vimfiles到.SpaceVim的符号链接，对于_vimrc则根据[用户配置](https://spacevim.org/cn/documentation/#%E7%94%A8%E6%88%B7%E9%85%8D%E7%BD%AE)创建环境变量SPACEVIMDIR值指向想要放置init.toml文件的位置如D:\Documents\Programming\vimhome，这里有一个待解决的问题见SpaceVim的[Issue #4021](https://github.com/SpaceVim/SpaceVim/issues/4021)。执行

```powershell
mklink "D:\Documents\Programming\emacshome\nvim" "D:\Documents\Programming\vimhome\.SpaceVim"
```

为NeoVim创建nvim到.SpaceVim的符号链接。