---
title: Solidworks出错导致solidworks意外退出 故障模 vcruntime140
categories: Software
tags:
  - solidworks
  - vcruntime
  - conisio
date: 2022-05-20 12:49:30
---

在 Solidworks 启动到 SOLIDWORKS PDM 时弹出窗口崩溃，显示出错,导致solidworks意外退出 故障模 vcruntime140:000064c0，尝试在 SOLIDWORKS Rx 中以 SOLIDWORKS 安全模式启动，分别以软件 OpenGL 模式启动 SOLIDWORKS 以及启动 SOLIDWORKS 而绕过工具/选项设定，都是在加载页面结束，主页面弹出时闪退。运行 SOLIDWORKS Rx 诊断，显示无异常。查看错误日志 SolidWorksPerformance.log，定位到：

> <AD2> "LOAD_ADDIN" "cslivelinksw.DLL" "LiveLink for COMSOL (6.0)" [0.79] "6.0.0.200" 48 NET 1625772126 16
> <SWEXH>
> <MINIDUMP> "374ed67f-3342-46e1-abc3-f2a1777a50b7" </MINIDUMP>
> <EXCEPTION> "C++ Exception"
> <AD5> auCEFBrowser_c::XSwCEFBrowser::get_URL;auCEFBrowser_c::XSwCEFBrowser::put_URL;auCEFBrowser_c::XSwCEFBrowser::get_URL;auCEFBrowser_c::XSwCEFBrowser::ExecuteJS;auCEFBrowser_c::XSwCEFBrowser::AddJSHandler;auCEFBrowser_c::XSwCEFBrowser::AddJSHandler;auCEFBrowser_c::XSwCEFBrowser::AddJSHandler;auAm_c::XDispatch::get_ActiveDoc;auAm_c::GetActiveDoc;
> <AD6> CHECKS_ENABLE_WITH_LOG;auCEFBrowser_c::XSwCEFBrowser::put_URL;
> <AD7> CHECKS_ENABLE_WITH_LOG;
> <AD2> "LOAD_ADDIN" "PDMSW.dll" "SOLIDWORKS PDM" [1.37] "30.2.0.51" 14 COM.new 1647645565 17
> <AD2> "LOAD_ADDIN" "SOLIDWORKSInspection.DLL" "SOLIDWORKS Inspection" [0.31] "2022.2.0.46" 80 NET 1647642840 18
> <AD2> "LOAD_ADDIN" "Dsgnchku.dll" "SOLIDWORKS Design Checker" [0.03] "30.2.0.46" 14 COM.new 1647642701 19
> <AD2> "LOAD_ADDIN" "cwaddinu.dll" "SOLIDWORKS CAM 2022" [1.53] "2022.2022.1.24" 14 COM.new 1643026890 1a
> <LOGINSTATUS_STARTUP>No user login detected</LOGINSTATUS_STARTUP>
> <UI_ENVIRONMENT>CMD:LOAD ATTR:TYPE=SWSystem ATTR:ID=1</UI_ENVIRONMENT>
> <UI_ICONCOLOR>CMD:LOAD ATTR:TYPE=SWSystem ATTR:ID=0</UI_ICONCOLOR>
> <UI_GESTUREGUIDE>CMD:LOAD ATTR:TYPE=SWSystem ATTR:ID=3</UI_GESTUREGUIDE>
> <RAWSTACK> KERNELBASE:7FFB44E0474C:0004474C,VCRUNTIME140:7FFB398064C0:000064C0,ConisioCAD:7FFAA602DB27:0006DB27,ConisioCAD:7FFAA5FD1105:00011105,KERNEL32:7FFB461C54E0:000154E0,ntdll:7FFB472A485B:0000485B;
> <UNHANDLED>
> </SWEXH>
> <AD2> "UNLOAD_ADDIN" "BendSequenceSwu.dll" "" [0.00] 0
> <PROCMEM> CMD:AppFinish ATTR:PageFileBytes=784224256 ATTR:PageFileBytesPeak=787259392 ATTR:PoolNonpagedBytes=275880 ATTR:PoolPagedBytes=4752928 ATTR:PrivateBytes=784224256 ATTR:VirtualBytes=76722728960 ATTR:VirtualBytesPeak=76727738368 ATTR:WorkingSet=988418048 ATTR:WorkingSetPeak=997429248 ATTR:AvailableReservesMask=7 ATTR:GDIHandlesTotal=10000 ATTR:GDIHandlesUsed=1845 </PROCMEM>
> <MEMORY> 557410, 1002283008, 996589568, 5069896, 4756904, 357048, 277192, 788602880, 795521024, 788602880 </MEMORY>
> <FT>1653015435
> <ELAPSED_SEC>12
> <FT_COMPUTED>1653015435
> <EOSL>0</EOSL>
> <RUNTIME>12</RUNTIME>
> <IDLETIME>0</IDLETIME>
> <AT>

推测是 ConisioCAD 导致的问题，于是==卸载 SOLIDWORKS PDM==，就可以成功启动 SOLIDWORKS，或如果能成功在 SOLIDWORKS 主页面闪退前管理插件，==取消勾选在启动时加载 SOLIDWORKS PDM 插件==，也可以解决该问题。如果一定要使用 SOLIDWORKS PDM，那么需要通过控制面板==卸载 SOLIDWORKS 程序，在 SOLIDWORKS 安装管理程序执行卸载前选项时，将所有卸载选项全部勾选，即可将“标准卸装”改为“完全卸装”。卸载完成后重启计算机，登录为 Administrator 账户，重新安装 SOLIDWORKS==，重启计算机激活后即可成功打开 SOLIDWORKS。或者可以尝试在安装时在摘要界面更改 SOLIDWORKS PDM 选项，选择您的 PDM 客户端类型不选择 SOLIDWORKS PDM CAD Editor：
