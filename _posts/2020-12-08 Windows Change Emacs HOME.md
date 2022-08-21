---
title: Windows更改Emacs的独立HOME路径的三种方法
categories: SuperUser
tags:
  - emacs
date: 2020-12-08 00:38:27
---

Emacs的HOME路径~默认为环境变量“HOME”的值（例如E:\Cadence\SPB_Data），若要为Emacs设置独立的HOME路径需要手动更改。经试验目前中文网络上所谓修改注册表值HKEY_LOCAL_MACHINE\SOFTWARE\GNU\Emacs\HOME=%emacs_dir%无效。

在原有HOME路径下新建.emacs文件并load file的方法经测试无效，例如把.emacs文件写入（参考[How do I set a different location for the dot emacs .emacs file on Windows 7?](https://emacs.stackexchange.com/questions/12881/how-do-i-set-a-different-location-for-the-dot-emacs-emacs-file-on-windows-7)）：

```lisp
(setq user-init-file "E:/Cadence/SPB_Data")
(setq user-emacs-directory "D:/Documents/Programming/emacshome")
(setq default-directory "D:/Documents/Programming/emacshome")
(setenv "HOME" "D:/Documents/Programming/emacshome")
(load user-init-file)
```

启动Emacs会报错：

> Warning (initialization): An error occurred while loading ‘E:/Cadence/SPB_Data’:
>
> File error: Cannot open load file, Permission denied, e:/Cadence/SPB_Data
>
> To ensure normal operation, you should investigate and remove the
> cause of the error in your initialization file.  Start Emacs with
> the ‘--debug-init’ option to view a complete error backtrace.

 （这里如果将user-init-file设置为其他路径，则该.emacs文件根本不会被使用）

**方法一（仍会在原HOME路径下创建空的.emacs.d文件夹）**：在emacs\share\emacs\site-lisp文件夹下新建site-start.el，写入：

```lisp
; E:\Program_Files\Emacs\x86_64\share\emacs\site-lisp
(defun set-home-dir (dir)
  (setenv "HOME" dir)
  (message (format "HOME location is %s" (getenv "HOME"))))
  (set-home-dir "D:/Documents/Programming/emacshome/")
```

启动Emacs即发现HOME目录改为新的目录（参考[Change ~ home folder location? (Windows)](https://www.reddit.com/r/emacs/comments/a6ka23/change_home_folder_location_windows/)）。

**方法二（仍会在原HOME路径下创建空的.emacs.d文件夹）**：执行

```lisp
SETX EMACS_HOME "D:\Documents\Programming\emacshome"
```


添加环境变量（这里是用户变量）或手动添加环境变量（系统变量也可以），这里的变量名EMACS_HOME可自己决定，在emacs\share\emacs\site-lisp文件夹下新建site-start.el，写入：

```lisp
; E:\Program_Files\Emacs\x86_64\share\emacs\site-lisp
(setenv "HOME" (convert-standard-filename (getenv "EMACS_HOME")))
```


启动Emacs即发现HOME目录改为新的目录（参考[Changing HOME directory only for Emacs (on Windows 7)]()）。

**方法三（不会在原HOME路径下创建空的.emacs.d文件夹）**：添加环境变量名为XDG_CONFIG_HOME，值为要更改为的路径。参考[49.4.4 How Emacs Finds Your Init File](https://www.gnu.org/software/emacs/manual/html_node/emacs/Find-Init.html#Find-Init)，Emacs会先按顺序寻找~/.emacs.el、~/.emacs或~/.emacs.d/init.el，Emacs还会寻找init.el的XDG兼容位置，默认为~/.config/emacs，该路径可被XDG_CONFIG_HOME环境变量覆盖，其值会替换默认XDG初始化文件名中的~/.config，然而，Emacs仍会优先寻找~/.emacs.d、~/.emacs和~/.emacs.el如果它们存在的话，所以需要先删除或重命名这些文件夹或文件以使用XDG位置。如果XDG位置和~/.emacs.d都不存在，则Emacs会创建~/.emacs.d。设置了XDG_CONFIG_HOME环境变量后，启动Emacs即发现HOME目录改为新的目录。

注意：使用这种方法时Emacs配置文件夹的名称必须为"emacs"，如果你使用GitHub上流行的Emacs配置例如[emacs.d](https://github.com/redguardtoo/emacs.d)需要将克隆下来的".emacs.d"文件夹手动重命名为"emacs"。