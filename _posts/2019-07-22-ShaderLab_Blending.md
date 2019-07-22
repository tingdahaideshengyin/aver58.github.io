---
layout:     post
title:      "ShaderLab: Blending"
subtitle:   "Shader入门"
date:       2019-07-20 18:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Shader
---

# ShaderLab: Blending
>Alpha混合
> [unity链接](https://docs.unity3d.com/Manual/SL-Blend.html)
>- Blend SrcFactor DstFactor  SrcFactor是源系数，DstFactor是目标系数
>- 最终颜色 = （Shader计算出的点颜色值 * 源系数）+（点累积颜色 * 目标系数）

属性  | 往SrcFactor，DstFactor 上填的值
---|---
one                   |     1
zero                  |     0
SrcColor              |     源的RGB值，例如（0.5,0.4,1）
SrcAlpha              |     源的A值, 例如0.6
DstColor              |     混合目标的RGB值例如（0.5，0.4,1）
DstAlpha              |     混合目标的A值例如0.6
OneMinusSrcColor      |     (1,1,1) - SrcColor
OneMinusSrcAlpha      |     1- SrcAlpha
OneMinusDstColor      |     (1,1,1) - DstColor
OneMinusDstAlpha      |     1- DstAlpha

- 实例：
1. Blend zero one：仅显示背景的RGB部分，无Alpha透明通道处理。
1. Blend one  zero：  仅显示贴图的RGB部分，无Alpha透明通道处理。 A通道为0即本应该透明的地方也渲染出来了。
1. Blend one  one：贴图和背景叠加，无Alpha透明通道处理。仅仅是颜色rgb数值的叠加更趋近于白色即（1，1，1）了。
1. Blend SrcAlpha  zero：仅仅显示贴图，贴图含Alpha透明通道处理。但是贴图中的透明部分，即下图黑色部分没有颜色来显示，因为源颜色乘以alpha值0，为0；而混合目标的颜色乘以zero 0，也是0。所以透明部分显示的颜色为（0，0，0）
1. Blend SrcAlpha  OneMinusSrcAlpha(最常用的透明混合方式。)： DstColor(new) = SrcAlpha * SrcColor + (1 - SrcAlpha) * DstColor(old)
> 最终颜色 = 源颜色 * 源透明值 + 目标颜色*（1 - 源透明值）

常见的混合类型  | 对应ps的混合模式
---|---
Blend SrcAlpha OneMinusSrcAlpha |正常,透明度混合
Blend OneMinusDstColor One      |柔和相加(soft Additive) 
Blend DstColor Zero             |正片叠底 (Multiply)相乘 
Blend DstColor SrcColor         |两倍相乘 (2X Multiply) 
BlendOp Min Blend One One       |变暗
BlendOp Max Blend One One       |变亮
Blend OneMinusDstColor One      |滤色 
Blend One One                   |线性变淡 