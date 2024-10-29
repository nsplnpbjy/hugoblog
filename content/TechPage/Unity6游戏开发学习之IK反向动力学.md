---
title: "Unity6游戏开发学习之IK反向动力学"
description: 
date: 2024-10-29T10:20:06+08:00
image: https://picsum.photos/800/600.webp?random=1071e9ae
math: 
license: 
hidden: false
comments: true
draft: false
toc : true
lastmod : 2024-10-29T10:20:06+08:00
tags: ["C#","游戏开发","Unity"]
categories: ["C#"]
---
# Unity6游戏开发学习之IK反向动力学

在Unity中，**反向动力学（Inverse Kinematics，简称IK）是一种动画技术**，它允许动画师通过指定某些关节（如手或脚）的最终位置来反向计算整个关节链的合理位置。这种技术在游戏开发中非常有用，尤其是在需要角色与环境进行物理交互的场景中，比如角色的手触摸到特定物体或脚步适应不平坦的地面。

## IK的实现步骤

要在Unity中实现IK，首先需要一个正确配置的人形Avatar骨骼。接着，创建一个Animator Controller，并为其添加至少一个动画。在Animator窗口的Layers面板中，点击层的齿轮设置图标，并勾选IK Pass复选框，这样才能启用IK功能。

接下来，需要编写一个脚本来处理IK，该脚本通常会命名为IKControl。在这个脚本中，可以使用Animator类的函数，如SetIKPositionWeight、SetIKRotationWeight、SetIKPosition、SetIKRotation等，来设置IK目标和权重。

## 例如，以下是一个简单的IK控制脚本示例：

~~~C#

using UnityEngine;

public class IKControl : MonoBehaviour {
protected Animator animator;
public bool ikActive = false;
public Transform rightHandObj = null;
public Transform lookObj = null;

void Start () {
animator = GetComponent<Animator>();
}

void OnAnimatorIK() {
if(animator) {
if(ikActive) {
if(lookObj != null) {
animator.SetLookAtWeight(1);
animator.SetLookAtPosition(lookObj.position);
}
if(rightHandObj != null) {
animator.SetIKPositionWeight(AvatarIKGoal.RightHand,1);
animator.SetIKRotationWeight(AvatarIKGoal.RightHand,1);
animator.SetIKPosition(AvatarIKGoal.RightHand,rightHandObj.position);
animator.SetIKRotation(AvatarIKGoal.RightHand,rightHandObj.rotation);
}
}
else {
animator.SetIKPositionWeight(AvatarIKGoal.RightHand,0);
animator.SetIKRotationWeight(AvatarIKGoal.RightHand,0);
animator.SetLookAtWeight(0);
}
}
}
}
~~~
在上述脚本中，ikActive变量用于控制IK是否启用。如果启用，脚本将设置角色的右手和头部的目标位置和旋转。如果未启用，脚本将重置这些部位的位置和旋转。

## 注意事项

在使用Unity的IK功能时，需要注意以下几点：

动画类型必须是Humanoid。

必须在Animator Controller的对应层中勾选IK Pass选项。

IK的设置和控制需要在OnAnimatorIK事件方法中进行。