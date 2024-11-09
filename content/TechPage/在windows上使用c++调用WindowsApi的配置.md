---
title: "在windows上使用c++调用WindowsApi的配置"
description: 
date: 2024-11-09T14:59:12+08:00
image: https://picsum.photos/800/600.webp?random=89accf10
math: 
license: 
hidden: false
comments: true
draft: false
toc : true
lastmod : 2024-11-09T14:59:12+08:00
tags: ["WindowsApi"]
categories: ["C++"]
---
# 在windows上使用c++调用WindowsApi的配置
## 编译器的选择
编译器一定要选择MSVC,这是微软自己搞的编译器，使用其他编译器可能会有意料之外的问题。
在这里有它的下载渠道[visual studio](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/),选择好你需要的工作集，它会自行给你下载安装好。
## 环境变量的配置
如果你不想用**visual studio**，这个东西功能太多太重了，我使用**VS Code**，那么你就要配置一些环境变量，否则你只能使用**Developer Command Prompt**这种东西，在**VS Code**的终端中不太好用。
我们打开**Developer Command Prompt**，输入命令**set**，找到其中的**INCLUDE** **LIB** **LIBPATH**比如我的：
~~~bash
INCLUDE=D:\msvc\VC\Tools\MSVC\14.41.34120\include;D:\msvc\VC\Auxiliary\VS\include;C:\Program Files (x86)\Windows Kits\10\include\10.0.26100.0\ucrt;C:\Program Files (x86)\Windows Kits\10\\include\10.0.26100.0\\um;C:\Program Files (x86)\Windows Kits\10\\include\10.0.26100.0\\shared;C:\Program Files (x86)\Windows Kits\10\\include\10.0.26100.0\\winrt;C:\Program Files (x86)\Windows Kits\10\\include\10.0.26100.0\\cppwinrt
JAVA_HOME=C:\Program Files\RedHat\java-17-openjdk-17.0.6.0.10-1\
LIB=D:\msvc\VC\Tools\MSVC\14.41.34120\lib\x86;C:\Program Files (x86)\Windows Kits\10\lib\10.0.26100.0\ucrt\x86;C:\Program Files (x86)\Windows Kits\10\\lib\10.0.26100.0\\um\x86
LIBPATH=D:\msvc\VC\Tools\MSVC\14.41.34120\lib\x86;D:\msvc\VC\Tools\MSVC\14.41.34120\lib\x86\store\references;C:\Program Files (x86)\Windows Kits\10\UnionMetadata\10.0.26100.0;C:\Program Files (x86)\Windows Kits\10\References\10.0.26100.0;C:\Windows\Microsoft.NET\Framework\v4.0.30319
~~~
把他们添加进系统环境变量里，也分别设置为**INCLUDE** **LIB** **LIBPATH**，其中根据你的操作系统去更改一下X86-》X64。
## 使用cl
MSVC的编译器操作命令是cl，比如`cl main.cpp`这样，你要把你的cl命令加入系统环境变量Path中去，比如：
~~~bash
D:\msvc\VC\Tools\MSVC\14.41.34120\bin\Hostx64\x64
~~~