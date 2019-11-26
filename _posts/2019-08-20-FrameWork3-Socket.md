---
layout:     post
title:      "【Unity游戏框架搭建】四、网络层框架(简单C#服务端搭建)"
subtitle:   "TeddyFrameWork"
date:       2019-08-20 11:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity
---

### 引言
1. 如果没有服务端，就没有网络，服务端是完成网络游戏的底层基础
2. 为了让我们游戏中能和其他人一起玩，就需要我们搭建一个网络层框架去与服务端通信，然后服务端收到消息，分发给客户端，完成客户端与客户端的联动。
3. 接下来，我们在这里我们实现了一个简单服务端搭建。

#### 一、创建简单服务端[Unity3D —— Socket通信(C#)](https://blog.csdn.net/linshuhe1/article/details/51386559)