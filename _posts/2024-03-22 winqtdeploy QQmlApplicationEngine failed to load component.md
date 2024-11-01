---
title: winqtdeploy后运行程序报错QQmlApplicationEngine failed to load component
categories: Build
tags:
  - qt
  - qt6
  - qml
date: 2024-03-22 01:05:04
---

为了将 Qt 程序打包成一个 EXE 文件，找到 Qt 6.6.2 (MSVC 2019 64-bit)（对于 MinGW 同理）并打开，在其命令行中输入
```cmd
cd E:\Documents\GitHub\build-<YourApp>-Desktop_Qt_6_6_2_MSVC2019_64bit-Release
windeploy .
```
后，不管是在 Qt Creator 内运行 exe 文件还是直接打开 exe 文件都会报错：
> QQmlApplicationEngine failed to load component
> qrc:/qt/qml/com/github/\<username>/main.qml:9:1: 类型 App 不可用
> qrc:/qt/qml/content/App.qml: 模块“QtQuick.VirtualKeyboard.Components”没有安装
> qrc:/qt/qml/content/App.qml: 模块“QtQuick.VirtualKeyboard.Layouts”没有安装
> qrc:/qt/qml/content/App.qml: 模块“QtQuick.VirtualKeyboard.Components”没有安装
> qrc:/qt/qml/content/App.qml: 模块“QtQuick.VirtualKeyboard.Layouts”没有安装
> 00:44:33: E:\Documents\GitHub\build-\<YourApp>-Desktop_Qt_6_6_2_MSVC2019_64bit-Release\\\<YourApp>.exe 退出，退出代码: -1
>  {1 ?} {2?}

参考 [QT failed to load components](https://forum.qt.io/topic/134335/qt-failed-to-load-components/5)，对于 QML 程序，应运行
```cmd
windeployqt --qmldir <location of qml folder> <location of exe>
```
对于本例，直接运行
```
windeployqt --qmldir qml YourApp.exe
```
即可。重新运行 exe 文件即可成功打开。