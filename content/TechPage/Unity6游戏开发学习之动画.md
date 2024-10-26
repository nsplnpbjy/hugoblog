---
title: "Unity6游戏开发学习之动画"
description: 
date: 2024-10-26T18:16:43+08:00
image: https://picsum.photos/800/600.webp?random=45dbe797
math: 
license: 
hidden: false
comments: true
draft: false
toc : true
lastmod : 2024-10-26T18:16:43+08:00
tags: ["C#","游戏开发","Unity"]
categories: ["C#"]
---
# Unity6游戏开发学习之动画
## 取得动画
由于我没有美术细菌，我的动画和模型都是从Unity的免费商店里面下载的。
## 利用好humanoid
**humanoid** 是一种通用的人体骨架标准，只要是按照此标准制作的模型骨骼和动画是可以通用的。
比如我们有一个拥有humanoid标准的模型叫bananaman，那么我们给它加上**Animator**动画控制器的时候，可以选择Animator的**Avatar**属性为适合的骨骼架构，通常你下载的模型里面会有骨骼。

如何检查一个动画是否是humanoid的？在你的project里面点击这个动画，检查它的Rig标签，如果它的Animation Type可以调为humanoid而不报错，就是可用的。
## Animator里面的layer和mask
在layer里点击小齿轮，可以看到**mask**选项，这是一个遮罩，可以只控制部分躯体实现某个动画，比如**边走边开枪**，就要用mask去除腿部动画，让一个layer可以控制全身跑步的同时，被mask罩住的上半身使用另一个layer控制的开枪动画。**控制上半身动画的另一个layer，一般要放一个空状态，不播放任何动画**，这样的话，当不特殊涉及上半身的动作发生时，上半身就会听从控制全身的layer来进行其他动画。

以下是AI关于怎么制作一个mask的回答：

**如果我想制作一个只控制小人腿部动作的动画，我该怎么做一个mask**

在 Project 窗口中右键，选择 Create > Avatar Mask。

将新创建的 Avatar Mask 命名为 LegsMask。

配置Avatar Mask：

选中 LegsMask，在 Inspector 窗口中展开骨骼层次结构，取消勾选所有上半身相关的骨骼，只保留下半身和腿部的骨骼勾选。

应用Avatar Mask：

打开 Animator 窗口，选中控制腿部动画的 Layer。

在 Layers 面板中，点击你要应用掩码的那一层，找到 Avatar Mask 选项，将 LegsMask 拖到这个位置。

## 用好blend tree
**blend tree** 可以在**animator**中右键创建，它可以非常方便地控制各向移动的状态切换

## 注意Has Exit Time参数
各个动画状态间转化的箭头，要注意其中的Has Exit Time，勾选这个参数，动画一定会播放完毕再进入下一个状态，根据游戏需要自行调整。