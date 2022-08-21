---
title: 彻底卸载Solidworks及Electrical以避免重新安装时出现1603、注册表权限错误或Installer未按预期运行
categories: CSDN补档
tags:
  - solidworks
  - sqlserver
  - installer
  - regedit
abbrlink: 27647
date: 2020-02-19 20:01:15
---

本来Solidworks的卸载问题相对不多，但是由于一波hp操作（在卸载程序中断过程中删除文件、注册表）导致卸载程序没有清理干净，导致后续产生了巨大问题，打开Solidworks安装程序发现许多选项是灰色并且未勾选的，之后还出现了其他问题。那么在这种极限状况下该如何挽救？

首先研究log日志文件，这非常关键，在这种安装尚未开始进行、"为支持部门保存日志"选项的窗口尚未弹出时，以Solidworks 2020 SP1.0为例，我们可以在C:\Users\<username>\AppData\Roaming\SOLIDWORKS\Installation Logs\2020 SP1.0中找到日志文件，我们之间进入子目录"Other Logs"中寻找“sldIMLog_xxxxx-xxxxx-xxxx_0000x.txt”文件中最近一次（或出现最大问题的那次），在子目录中的sldIMLog日志比上一级目录中的同名文件记录更详尽。此外，同样在Other Logs中我们找到GetSourceInfo_xxxxx-xxxxx-xxxx-xxx.xml文件，最好通过IE等支持XML查看的浏览器或XML查看器查看，自动缩进看的好看一点。开始工作：

1.对于Solidworks Electrical用户，先看结尾处的注意事项。

2.如果之前未在控制面板使用Solidworks自己的卸载程序卸载，请先进行此操作，即使你知道TeighaX、Bonjour等软件是它的附加组件，也无需卸载它们（除非已经安装了比Solidworks安装程序自带的更高版本），**注意不要卸载Visual C++ Redistributable、.NET Framework等（除非你之后安装的安装程序已经明确指出安装问题是由它们引起）！卸载重装往往会造成非常严重、不可挽回的问题**。

3.确认卸载操作完成后，下载并安装Windows Installer Clean Up，尽管该工具是为Windows XP设计，对于Windows 10的MSI许多地方显得捉襟见肘，但是目前尚无代替工具。用管理员打开后，找到带Solidworks字段的几个程序Remove。如果这一步骤与后续步骤顺序颠倒导致问题，请转至步骤6参考。

该工具会出现兼容性问题，点击Remove运行一段时间后会重启一个窗口最后一行显示Removed /Features并持续占用CPU，这种情况下只要直接关闭该窗口，再次Remove运行一段时间后重启窗口最后一行显示Removed /Patches，然后再重复关闭、Remove的操作相继remove media、net等直到不再弹出为止，发现列表中要删除的程序已经消失。如果未找到Solidworks相关程序，反而找到一些(All Users)后名称与版本号为空或名称为空的程序，很可能是已损坏的Solidworks相关程序，也需要将其Remove！事实上，即使它们与Solidworks无关，它们也已经损坏，Remove不会对软件正常运行产生影响。

删除X:\Program Files\SOLIDWORKS Corp(X代表盘符C/D/E等)、X:\ProgramData\SOLIDWORKS Electrical，以及C:\Program Files、C:\Program Files (x86)及其子目录Common Files中的Solidworks或Solidworks Shared文件夹，以及C:\WINDOWS\Solidworks（经常被忽略的点），以及C:\Users\<username>\AppData的子目录Local、LocalLow、Roaming中含有Solidworks的文件夹。事实上，X:\ProgramData\SOLIDWORKS Data作为存放异孔型向导Toolbox的目录可以不必删除，在下次执行Solidworks安装程序时可以手动设置该目录，安装程序会自动更新其内容。如果你使用_SOLIDSQUAD_团队的激活工具，尽量不要删除C:\SolidWorks_Flexnet_Server，因为Cadence Allegro、Autodesk等软件激活会共用该Server的服务等，由于该团队的工具可以延续之前保留的激活状态无需激活，所以尽量不要删掉Flexnet相关文件、服务、注册表等，否则共用该服务的软件激活状态会出现异常，造成更大损失。

4.Win+R运行输入regedit打开注册表编辑器，删除下列注册表项：

\* 计算机\HKEY_CURRENT_USER\Software\SolidWorks

\* 计算机\HKEY_LOCAL_MACHINE\SOFTWARE\SolidWorks

\* 计算机\HKEY_LOCAL_MACHINE\SOFTWARE\SolidWorks Corporation

\* 计算机\HKEY_USERS\S-1-5-xx(-x...)\Software\SolidWorks【可能在S-1-5-21-...】

5.使用批量的注册表管理工具（推荐Registrar Registry Manager）搜索所以带Solidworks的注册表项，全部搜索出后，由于Solidworks会与Autodesk、ANSYS、COMSOL、Altium Designer、Microsoft Office等软件交互，删除时应避开带有这些字段的注册表项和值。然后再搜索eDrawings等字段补遗。

6.运行Solidworks安装程序到安装选项选择安装组件那一页，如果发现有些尚未勾选的选项变灰无法勾选，退出安装程序，检查之前找到的txt日志文件，发现最后提示

```
[I    45 1 entitleme..ecker.cpp(1934) 10:42:10] {ID #55700} $S10:42:10
Info	Entitlement	45	55700	"所隐藏的产品(无法选定):{0: SOLIDWORKS Installation Manager}"
```

还有另外44条类似的提示，并最终汇总给出"可以选定的产品"、"用户已授权安装的产品"、"要进行新安装(不是升级)的产品"、"已安装并要保留的产品"、"要升级的产品(带有升级的版本)"、"要删除的产品"。深入查看，发现许多类似于

```
[I     0 1 entitleme..ecker.cpp(2746) 10:42:10]
SetFeaturesEntitlement() report for PlugIns [=>PlugIns, p=Composer, 6 of 8, children=0]:
[selected=1, selectable=1, mode=1], [ent=1], skip=0, clf=0, addins=0, asel=1
```

的提示，我们发现，如果skip为1，那么selected与selectable均为0；如果asel为1，则selected, selectable, mode均为1。如果clf为1，则ent为0而asel为1。继续向上翻看，我们发现多条下列提示：

```
[I     0 2     msiinstaller.cpp(2804) 10:42:08]
SKIPPING INSTALL CONDITION - installed version OK (productcode={......}
```

以及

```
[I     0 2     msiinstaller.cpp(2829) 10:42:08]
INSTALL CONDITION Product NOT detected (upgradecode={......}, msiversion=14.16.27012
[I     0 2  clientinstaller.cpp(3538) 10:42:08]
INSTALL CONDITION IS FALSE!
[E     0 1      projectdata.cpp(2485) 10:41:07]
WARNING! IsSelected called on product (SWKorean) with no actions - using hidden m_bSelected
```

这意味着这些productcode对应的组件的Installer被detect到而被skip，而且还有Uninstall的Registry Key丢失，所以我们想到要删除它们。打开之前关于GetSourceInfo的xml日志文件，用注册表管理器打开"计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\Folders"，向下找到值为"C:\WINDOWS\Installer\{…}的范围内，在XML文件中搜索/ProductCode，将搜索出的SolidWorks、SWSupportedLanguages等的Product的Code在这个范围内的值对应{}括号内的值删掉，如SolidWorks对应3F4681F3打头的Code，将该值从Folders项中删去。值得注意的是，ProductCode其后的UpgradeCode有时也有用，不过这里可以忽略。此外，一些名为SWKorean之类的语言包类Product可以跳过（除了你先前安装的其他语言如Chinese、Chinese Simplified），此外，还可以跳过一些附加组件如Bonjour、TeighaX、Microsoft_VSTA、Microsoft_VBA等，**不要动MSSQL相关注册表**。大致清理注册表（即步骤4中的注册表），重启安装程序，发现问题解决。

7.目前可以勾选了，设置好后开始安装，却在安装Solidworks主程序遇到了注册表权限问题，接着出现Installer未按预期运行而中断，这是老生常谈的问题了。检查txt日志文件，跳过大段无用的[E  157 1   msiinstaller.cpp(3337) 13:33:58] $S13:33:58   Error   Status发现提示[I   0 3  clientinstaller.cpp(7093) 13:33:58] ::MsiInstallProduct returned 1603。这时网上的删除VSTA注册表等种种怪招自然是没用的，还有些给出的卸载方案根本卸载不干净，还有的则会造成破坏。我们记下安装程序给出的注册表位置在Unknown\……\……，两个省略号都是大段十六进制code，在注册表中搜索第一个code得到3个结果，其中一个定位到计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData\S-1-5-18\Components\，发现其子项出现严重权限问题打不开，也更改不了所有者权限，参考我的另一篇文章[Windows 10无法打开注册表 由于某个错误无法打开该密钥(详细信息:拒绝访问)且无法在注册表上设置新的所有者拒绝访问的解决方案](https://blog.csdn.net/yihuajack/article/details/104395132)，更改"Component"项的所有者并选定替换子容器和对象所有者、勾选窗口最下方的使用可从此对象继承的权限项目替换所有子对象的权限项目。大致清理注册表（即步骤4中的注册表），重启安装程序，发现问题解决。再次提示记得成功后恢复"Component"项及其子项的所有者为"SYSTEM"并继承Administrators的权限以免对系统和其他软件造成影响。

------

**针对Solidworks Electrical用户：**

**注意：如果使用Solidworks安装媒介自带的SQL Server Express新建实例localhost\TEW_SQLEXPRESS来安装Solidworks Electrical，切勿对该SQL Server进行删除服务、注册表或卸载软件等操作！**SQL Server是一个远比其他软件复杂的系统，如果其被损坏产生安装问题大多几乎无解，且对系统造成广泛、不可逆转的损害，而且原先的安装媒介往往丢失、不兼容、失效，修复能力很有限，往往只能重装系统。

i)如果已经误操作而产生不可逆转的后果，只能将原先的TEW_SQLEXPRESS搁置，新建实例重命名为例如"TEW_SQLEXPRESS1"继续。注意实例名有长度限制。

ii)如果尚未损坏SQL Server服务、注册表、软件、实例等，就不要修改、删除它们，因为SQL Server的实例高度独立，Solidworks的修改并不对其造成多大影响。如果后续SQL Server确实报错，最好通过SSMS(SQL Server Management Studio)或SQL Server安装程序安全删除该实例，并清理服务，但是仍然不要修改SQL Server的注册表或卸载其软件。如果仍然报错，请转至选项i)。

事实上，如果你以安装高于2014版本（到Solidworks 2020为止）的SQL Server或高于Express的版本如Developer、Enterprise、Professional等，你可以使用自己的SQL Server新建localhost下或你的计算机名下(例如名为DESKTOP-XXXXXXX\XXX)的实例在Solidworks安装程序设定中用于Solidworks Electrical。你可以选择Windows身份验证登录方式，也可以选择SQL Server身份验证登录方式自己设定密码，二者经测均可。

（20200519更新：注意采用已安装的SQL Server实例用于Solidworks Electrical时，若安装的是默认实例MSSQLSERVER，则服务器名称应为你的**计算机名**。）