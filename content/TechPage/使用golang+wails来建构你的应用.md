---
title: "使用golang+wails来建构你的应用"
description: 
date: 2024-10-23T16:20:39+08:00
image: https://picsum.photos/800/600.webp?random=8e8de125
math: 
license: 
hidden: false
comments: true
draft: false
toc : true
lastmod : 2024-10-23
tags: ["桌面应用"]
categories: ["wails",""go""]
---

# 什么是wails?
[wails](https://wails.io/zh-Hans/)是一个go语言的框架，它可以用来构建跨平台的桌面应用。

简单来说就是**js做界面，go做业务**

**本质上还是去皮浏览器那一套，但是他可以方便地调用go的代码**
## 依赖
Wails 有许多安装前需要的常见依赖项：
Go 1.18+
NPM (Node 15+)

## 安装 Wails
运行 `go install github.com/wailsapp/wails/v2/cmd/wails@latest` 安装 Wails CLI。

## 系统检查
运行 `wails doctor` 将检查您是否安装了正确的依赖项。 如果没有，它会就缺少的内容提供建议以帮助纠正问题。

## 找不到wails命令？
如果你找不到wails命令，极大的可能是**检查 "~/go/bin" 没有在你的 PATH 变量中： echo $PATH | grep go/bin**
如果在**Windows**下，你可以编辑环境变量，在系统变量的Path里面加入你的**go/bin**位置
如果在**ubuntu**下，在你所使用的命令行配置文件里写入
~~~bash
export GOROOT=你的/go路径
export GOPATH=你的第三方包体路径
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
~~~

# 怎么使用wails？

## 创建一个wails项目
我们使用如下代码创建一个前端为react框架的项目
~~~bash
wails init -n myproject -t react
~~~
## 开发你的前端页面
现在我们的项目里有一个**frontend**文件夹，这里面就是传统的react项目，只需打开**src**文件夹，像做一个普通的react页面一样写代码即可。
**不要动wailsjs文件夹里面的东西，他是wails框架自动生成的**
## 开发你的后端代码
现在关注我们项目里的**app.go**和**main.go**

### app.go
我们关注这个函数
~~~go
func (a *App) startup(ctx context.Context){}
~~~
这个函数是app被启动后调用的第一个函数（可以在main.go里面修改），如果你想在程序启动时完成一些操作，请加在这里面。

其他函数就需要你自己去完成，好让前端代码调用。比如我写的这个函数：
~~~go
func (a *App) VMT(text string) string {
	return VMTB.VoiceMultiTalk(text)
}
~~~
它有一个特点，要规定**App**类型才能调用，这样才能被前端正确绑定。

如果你自行创建了**bpp.go**，那么同理，里面的函数要规定**Bpp**类型才能调用。

现在我们来将这些**app**、**bpp**与前端绑定。

### main.go
我的main.go如下：
~~~go
package main

import (
	"embed"

	"github.com/wailsapp/wails/v2"
	"github.com/wailsapp/wails/v2/pkg/options"
	"github.com/wailsapp/wails/v2/pkg/options/assetserver"
)

//go:embed all:frontend/dist
var assets embed.FS

func main() {
	// Create an instance of the app structure
	app := NewApp()

	// Create application with options
	err := wails.Run(&options.App{
		Frameless: true,
		Title:     "voicerobot",
		Width:     1024,
		Height:    768,
		AssetServer: &assetserver.Options{
			Assets: assets,
		},
		BackgroundColour: &options.RGBA{R: 27, G: 38, B: 54, A: 1},
		OnStartup:        app.startup,//规定要用哪个结构体的哪个函数在启动时被调用
		Bind: []interface{}{
			app,//把你的结构体放在这里可以完成与前端的绑定，以让前端使用结构体中的函数
		},
	})

	if err != nil {
		println("Error:", err.Error())
	}
}
~~~
## 在你的前端中调用APP或者BPP里面的代码
比如我们的项目，有个**App.jsx**，我现在要在里面调用**app.go**里面的代码。

使用`import { funcApp1, funcApp2 } from "../wailsjs/go/main/App"`来引入你的js代码中，其中 **../wailsjs/go/main/App** 这个路径是wails自动生成的，你不要去手动改变它。

同理，使用`import { funcBpp1,funcBpp2 } from "../wailsjs/go/main/Bpp"`来将你的Bpp结构体中的函数引入。

接下来，你就可以在你的js代码里面直接使用go函数了。