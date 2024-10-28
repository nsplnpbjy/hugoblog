---
title: "Unity6游戏开发学习之碰撞"
description: 
date: 2024-10-26T18:05:08+08:00
image: https://picsum.photos/800/600.webp?random=20addd08
math: 
license: 
hidden: false
comments: true
draft: false
toc : true
lastmod : 2024-10-26T18:05:08+08:00
tags: ["C#","游戏开发","Unity"]
categories: ["C#"]
---
# Unity6游戏开发学习之碰撞
## 首先是碰撞的条件
碰撞的两个对象，**两个对象都** 必须有碰撞体 **Collider** ，这个碰撞体可以是**Box Collider**也可以是其他的比如**Capsule Collider**和**Mesh Collider**

然后碰撞的 **其中一个** 对象必须有 **Rigidbody** 即刚体，是Unity引擎用来计算物理效果的对象。

然后两个对象才能发生碰撞。

## Mesh Collider的条件
Mesh Collider的网格细分比较多，效果好，但是较为耗费性能。

另外，根据Unity的官方手册，**Mesh Collider**与 **Rigidbody** 一起使用时，必须勾上**Convex** ，否则不能正常工作。此选项会粗略地给出碰撞体，降低性能开销，但是没那么精细了。

## Rigidbody的安排

我们**一般不在主角身上使用Rigidbody**，因为这里面有很多物理引擎引出的问题，解决起来不容易。
但如果你的玩法很新，需要一个Rigidbody安排在主角身上，那就尝试使用它。
让其他可碰撞的物体拥有其他碰撞体。

## Rigidbody里面的Collision Detection是碰撞体检测

有多个选项，分别适应低速、高速等物体碰撞

## 碰撞后的效果
### OnCollisionEnter
比如我们有子弹，我们要在刚体 **Rigidbody** 勾选上 **Is Kinematic** 选项，此选项会让你的对象不会被其他物理效果影响，但自身能影响其他对象的物理效果，子弹特别适用。**手雷不行，手雷涉及到弹墙壁**
我们给子弹加一个脚本，脚本里面我们去覆写一个方法 **void OnCollisionEnter(Collision collision)**

覆写这个方法可以利用collision去获得被撞到的对象比如：
~~~C#
void OnCollisionEnter(Collision collision)
    {
        // 检查是否碰到小人
        if (collision.gameObject.CompareTag("Enemy"))
        {
            bananaman bananaman = collision.gameObject.GetComponent<bananaman>();
            bananaman.TakingDamage();
        }
        // 销毁子弹
        Destroy(gameObject);
    }
~~~
这是用子弹去调用被击物体的方法，在工业上，通常是让被击物体主动调用**void OnCollisionEnter(Collision collision)**检查自身是否被击，然后去进行下一步处理，以及销毁子弹：
~~~C#
using UnityEngine;

public class EnemyController : MonoBehaviour
{
    private Animator animator;

    void Start()
    {
        animator = GetComponent<Animator>();
    }

    void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("Bullet"))
        {
            animator.SetTrigger("isHurt");
            // 销毁子弹
            Destroy(collision.gameObject);
        }
    }
}
~~~
### OnTriggerEnter
还是比如我们有子弹，击中了某个被设置了**trigger**的collider，被击物体就会调用OnTriggerEnter、OnTriggerStay之类的事件，通常用来区分被击部位。
比如头部和胸部各有collider，collider都被设置为**trigger**，那么子弹击中头部就会触发头部的OnTriggerEnter，你在头部写的脚本就可以让角色死亡，同理，胸部的就设置掉多少血。