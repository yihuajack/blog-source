---
title: 为PowerShell和PowerShell Core 7设置不同的profile配置文件路径
categories: CSDN补档
tags: powershell
abbrlink: 33820
date: 2020-03-14 21:34:24
---

在体验PowerShell 7并配置profile文件后发现，不同于PowerShell 5.1，使用ScreenFetch命令会报错：

```
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:65
Line |
  65 |      return $env:USERNAME + "@" + (Get-WmiObject Win32_OperatingSystem …
     |                                    ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
 
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:70
Line |
  70 |      return (Get-WmiObject Win32_OperatingSystem).Caption + " " +
     |              ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
 
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:76
Line |
  76 |      return (Get-WmiObject  Win32_OperatingSystem).Version;
     |              ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
 
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:81
Line |
  81 |      $Uptime = ((Get-WmiObject Win32_OperatingSystem).ConvertToDateTim …
     |                  ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
 
InvalidOperation: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:86
Line |
  86 |      $FormattedUptime =  $Uptime.Days.ToString() + "d " + $Uptime.Hour …
     |      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | You cannot call a method on a null-valued expression.
 
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:107
Line |
 107 |      $monitors = Get-WmiObject -N "root\wmi" -Class WmiMonitorListedSu …
     |                  ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
 
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:133
Line |
 133 |      return (((Get-WmiObject Win32_Processor).Name) -replace '\s+', '  …
     |                ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:138
Line |
 138 |      return (Get-WmiObject Win32_DisplayConfiguration).DeviceName;
     |              ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:143
Line |
 143 |      $FreeRam = ([math]::Truncate((Get-WmiObject Win32_OperatingSystem …
     |                                    ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:144
Line |
 144 |      $TotalRam = ([math]::Truncate((Get-WmiObject Win32_ComputerSystem …
     |                                     ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
RuntimeException: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:146
Line |
 146 |      $FreeRamPercent = ($FreeRam / $TotalRam) * 100;
     |      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Attempted to divide by zero.
RuntimeException: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:148
Line |
 148 |      $UsedRamPercent = ($UsedRam / $TotalRam) * 100;
     |      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Attempted to divide by zero.
InvalidOperation: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:151
Line |
 151 |      return $UsedRam.ToString() + "MB / " + $TotalRam.ToString() + " M …
     |             ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | You cannot call a method on a null-valued expression.
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:158
Line |
 158 |      $NumDisks = (Get-WmiObject Win32_LogicalDisk).Count;
     |                   ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:189
Line |
 189 |          $DiskID = (Get-WmiObject Win32_LogicalDisk).DeviceId;
     |                     ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:191
Line |
 191 |          $FreeDiskSize = (Get-WmiObject Win32_LogicalDisk).FreeSpace
     |                           ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
Get-WmiObject: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:195
Line |
 195 |          $DiskSize = (Get-WmiObject Win32_LogicalDisk).Size;
     |                       ~~~~~~~~~~~~~
     | The term 'Get-WmiObject' is not recognized as the name of a cmdlet, function, script file, or operable
     | program. Check the spelling of the name, or if a path was included, verify that the path is correct
     | and try again.
InvalidOperation: C:\Users\<username>\Documents\PowerShell\Modules\windows-screenfetch\1.0.2\Data.psm1:215
Line |
 215 |              $FormattedDisk = "Disk " + $DiskID.ToString() + " Empty";
     |              ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | You cannot call a method on a null-valued expression.
                         ....::::
                 ....::::::::::::       OS: Dell Inc. 03RV2M
        ....:::: ::::::::::::::::       Kernel: PowerShell 7.0.0
....:::::::::::: ::::::::::::::::       Uptime: DWM
:::::::::::::::: ::::::::::::::::       Motherboard: Segoe UI
:::::::::::::::: ::::::::::::::::       Shell:
:::::::::::::::: ::::::::::::::::       Resolution:
:::::::::::::::: ::::::::::::::::       Window Manager:
................ ................       Font:
:::::::::::::::: ::::::::::::::::       CPU:
:::::::::::::::: ::::::::::::::::       GPU
:::::::::::::::: ::::::::::::::::       RAM:
'''':::::::::::: ::::::::::::::::
        '''':::: ::::::::::::::::
                 ''''::::::::::::
                         ''''::::
 
 
```

所以我们想要为PowerShell和PowerShell Core设置不同的profile配置文件路径，方法如下：

将C:\Users\<username>\Documents\PowerShell中的Microsoft.PowerShell_profile.ps1复制到C:\WINDOWS\System32\WindowsPowerShell\v1.0中，则现在：

C:\WINDOWS\System32\WindowsPowerShell\v1.0\Microsoft.PowerShell_profile.ps1是PowerShell 5.1的配置文件；

C:\Users\<username>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1是PowerShell 7的配置文件。

注意，如果你之前的配置文件名称是profile.ps1，则你需要将其文件名更改为Microsoft.PowerShell_profile.ps1。

这种方法有一个bug，如果你在前者的profile文件中加入ScreenFetch而后者不加入，尽管PowerShell 7不再执行ScreenFetch命令，但是PowerShell 5.1会将ScreenFetch执行两遍，我们只好将前者的profile文件中的ScreenFetch命令也删去，则PowerShell 5.1只执行一次ScreenFetch。

另外，更改PowerShell的profile文件的路径可使用命令

```
$profile = "NewLocation\profile.ps1"
```