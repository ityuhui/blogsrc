title: "使用 Visual Studio 2015 编写Windows平台下的OpenGL程序"
date: 2016-03-01 09:49:13
tags:
---
OpenGL是跨平台的三维图形库，本文介绍如何使用Visual Studio ( VC++ ) 2015搭建Windows平台下的OpenGL开发环境

## 无需下载

笔者使用的Windows 7 Professional，在安装了Visual Studio 2015之后，默认已有OpenGL的库文件和头文件。

## 下载freeglut

freeglut是一个小型的图形工具库，用于提供创建和关闭窗口，Windows事件循环，响应鼠标键盘事件等功能，特别适宜于编写OpenGL小型程序，有了它，我们无需再使用MFC，QT等大型的图形框架（GUI Framework）。

> 到OpenGL的官网，找到freeglut的下载地址，里面有源代码和预编译二进制两种包，为了简单，笔者下载了预编译好的二进制包。

## 建立工程

### 1. 新建工程

打开VS2015，新建一个Win32 Console Application，名字为openglsam, 其他选择默认。

### 2. 设置正确的Solution Platforms

将工具栏上Solution Platforms设置为x64

### 3. 配置头文件和库文件依赖

打开Project --> openglsam Properites --> C/C++ --> General --> Additional Include Directories
加入freeglut里的include目录

打开Project --> openglsam Properites --> Linker --> General --> Additional Library Directories
加入freeglut里的lib/x64目录

打开Project --> openglsam Properites --> Linker --> Input --> Additional Dependencies
加入freeglut.lib

### 4. 拷贝动态库文件

将freeglut/bin/x64/freeglut.dll拷贝到项目openglsam目录 openglsam\x64\Debug下

### 5. 编写代码

https://github.com/ityuhui/openglsam

## [评论](https://github.com/ityuhui/BlogComments/issues)
