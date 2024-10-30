---
title: "Unity6游戏开发学习之人物攻击以及受击"
description: 
date: 2024-10-30T14:35:10+08:00
image: https://picsum.photos/800/600.webp?random=6fc93a2b
math: 
license: 
hidden: false
comments: true
draft: false
toc : true
lastmod : 2024-10-30T14:35:10+08:00
tags: ["C#","游戏开发","Unity"]
categories: ["C#"]
---
# Unity6游戏开发学习之人物攻击以及受击
**这是一个计划，后续随着开发实践将做出修正改动**
## 人物模型和动画
人物模型和动画是从[Unity Asset store](https://assetstore.unity.com/)下载的。
人物模型的骨架**RIG**是**humanoid**标准的，动画也是，可以适配套用。
## 人物移动
我下载的人物移动的动画有八个移动方向，有走、跑、冲刺等行为。

人物的移动动画使用**blend tree**控制，并且不使用**root motion**，而是使用刚体**Rigidbody**来控制人物旋转和移动。

通过脚本接收输入，然后改变刚体的速度来控制移动方向，要注意把速度向量从**世界坐标**的向量改为**角色坐标**的向量。

旋转要注意用一个值把鼠标输入的角度存起来，并且在每帧加减新输入的值，使用刚体的**MoveRotation**方法来旋转刚体。

跳跃就是给角色一个Y轴上的冲击力**impulse force**。

地面检测使用球形检测**Physics.SphereCast**来判断人物有没有着地。

一切涉及刚体的物理状态更新，比如改变刚体速度，旋转刚体，给刚体施加力，都应该在**FixUpdate()**里面进行，这是物理步长。而不是在帧步长**Update()**里面更新，否则会出现意想不到的问题。
## 人物攻击和受击
### 给人物设置受击体
比如我们给人物的胸部增加一个空对象碰撞体 **Box Collider**,并给空对象的模型一个**Tag**，就叫**chest**。

写一个通用受击脚本，这个脚本通过检测本体的**Transform** 来确定自身 **tag**，根据不同的**tag**来确定自身是**chest** **head** **belly**哪个部位，把它放在**startup**里面来检测。

我们把所有可受击的东西，比如**chest**这个空对象的layer设置为**hurtful**。

脚本提供一个受击方法比如**GetHit**，这个方法将在被对方**攻击检测**到之后调用，根据攻击的部位和受击的部位来决定伤害值，然后在里面再调用角色的生命值加减方法和角色的受击动画方法，具体的实现方法各种各样。

### 给人物设置攻击检测
**我们在动画中进行攻击检测**，比如出拳的动画，在手臂完全伸直的那一帧，增加一个事件**Event**，设置好要触发的方法名，比如**LeftStraightPunch**，之后在其他的脚本里去实现**LeftStraightPunch**，要注意必须是**public void**的，这个方法会在动画的这一帧触发。

**LeftStraightPunch**方法的脚本放去左手中指的模型上，放在这里检测效果好一点，当然也可以不放在中指上。

现在我们要用**Physics.SphereCast**来检测碰撞，检测的layer就是**hurtful**。撞到就调用被撞对象的**GetHit**方法。

## 流程
A角色开始攻击动画 =》 攻击动画进行到Event帧 =》 触发LeftStraightPunch方法 =》 Physics.SphereCast攻击检测 =》 检测到的Collider依次调用GetHit =》 GetHit依据传入的参数和自身的参数计算伤害值 = 》 GetHit调用角色生命值加减方法（比如叫HP Control，挂在角色身上）=》 GetHit调用角色受击动画方法