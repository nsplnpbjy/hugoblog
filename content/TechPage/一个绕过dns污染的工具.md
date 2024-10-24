---
title: "一个绕过dns污染的工具"
description: 
date: 2024-10-24T11:19:03+08:00
image: https://picsum.photos/800/600.webp?random=8407e0bb
math: 
license: 
hidden: false
comments: true
draft: false
toc : true
lastmod : 2024-10-24T11:19:03+08:00
tags: ["go","dns","github"]
categories: ["go"]
---
# 这是一个绕过dns污染的工具
我们现在有很多网站，因为不可言说的原因，虽然没被墙，但是因为DNS污染，所以时常会访问失败。

比如 **github.com** 经常会失败，给我们的push和pull都造成了困难。

## 下载
我开发了一个软件，针对Windows系统，我们点击这里下载 [hostalter](../attach/hostalter.exe)

## 自行编译或者修改
当然如果你不放心，可以自行编译，这里是源码 [hostalter](https://github.com/nsplnpbjy/hostsAlter)

## 使用
以管理员身份运行的同时带上域名参数 比如我用**powershell**打开
~~~bash
./hostalter.exe github.com
~~~
就会把github.com的最快ip修改进你的hosts，同时删除已有的github.com记录。
更换域名参数可以访问其他被DNS污染的网站。