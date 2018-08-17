---
title: QT-环境搭建-Failed to retrieve MSVC Environment
date: 2018-08-17 16:48:28
tags: 
- qt
- env
---

>出错环境：Win10 + QT 5.11.1 + MSVC2016_32bit (from: MSVC 2017 Community)

QT installer 提示有新的版本，emmmmm...点完确认更新之后的 0.1s 后我就后悔了，Windows 这么高深莫测的环境变量和注册表可能不是我驾驭得了的...然后等更新完炸了，好好的 QT + MSVC 编译环境全炸了！

<!-- more -->

*Failed to retrieve MSVC Environment！！！mother f...*

掏出万能的 Stack Overflow https://stackoverflow.com/questions/46951357/qt-creator-on-win-10-failed-to-retrieve-msvc-environment

注意到这段回答，和这位小哥是缘分：
>The comments above helped to find the likely source. I couldn't "fix" it, but get around it by opening the Qt developer's cmd prompt, load the appropriate vsvarsall.bat file and the run qtcreator from the same cmd prompt. QtCreator will subsequently be unresponsive for over a minute after it's started, but then it'll be fine.

### 解决方案
```
# 启动 cmd
WIN + R 输入 cmd

# 跳转至 MSVC 环境 XX 目录
cd C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\

# 等加载完 msvc 的环境，会弹出一个新的 cmd
start vcvarsall.bat amd64_x86

# 在新弹出来的 cmd 中切换到 QtCreator bin 目录
cd C:\Qt\Tools\QtCreator\bin

# 手动启动 QtCreator
start qtcreator.exe

# done
```

### 补充说明

在查询解决方案的过程中有看到官方论坛里评论可能是 cmd 执行过长引起的：https://bugreports.qt.io/browse/QTCREATORBUG-19099

具体原因还不是很清楚，这个方案只是能启动一个正常工作的 QtCreator
