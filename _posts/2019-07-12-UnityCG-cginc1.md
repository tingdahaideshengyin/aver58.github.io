---
layout:     post
title:      "【接口】Unity中UnityCG.cginc中常用的函数"
subtitle:   "Shader入门"
date:       2019-07-12 15:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Shader
---

# Unity中UnityCG.cginc中常用的函数【接口】

## 摄像机方向
摄像机方向（视角方向） | detail
---|---
【Obsolete】float3 WorldSpaceViewDir (float4 v)       | 输入一个模型空间中的顶点坐标    ==》输出（世界空间）从这个点到摄像机的观察方向；
float3 UnityWorldSpaceViewDir (float4 v)              | 世界空间的顶点坐标              ==》世界空间从这个点到摄像机的观察方向；
float3 ObjSpaceViewDir (float4 v )                    | 模型空间中的顶点坐标            ==》模型空间从这个点到摄像机的观察方向；

## 光源方向
光源方向 | detail
---|---
float3 WorldSpaceLightDir ( float4 v )     |(ForwardBase Only,not normalized)模型空间中的顶点坐标  ==》世界空间中从这个点到光源的方向
float3 UnityWorldSpaceLightDir ( float4 v ) |(ForwardBase Only,not normalized)世界空间中的顶点坐标 ==》世界空间中从这个点到光源的方向
float3 ObjSpaceLightDir ( float4 v )    	 |(ForwardBase Only,not normalized)模型空间中的顶点坐标    ==》模型空间中从这个点到光源的方向；

## 方向转换
方向转换 | detail
---|---
float3 UnityObjectToWorldNormal ( float3 norm )     |法线方向矢量 模型空间==》世界空间
float3 UnityObjectToWorldDir ( float3 dir )      |方向矢量 模型空间==》世界空间
float3 UnityWorldToObjectDir ( float3 dir )     |方向矢量 世界空间==》模型空间

# 顶点计算
顶点计算 | detail
---|---
TRANSFORM_TEX     | 缩放和偏移2D纹理的UV，对纹理坐标进行变换，对应材质面板的Tiling和Offset调节项


## Data结构在UnityCG.cginc
Data结构在UnityCG.cginc　 | detail
---|---
appdata_base |顶点着色器输入与位置,法线,纹理坐标
appdata_tan：|顶点着色器输入与位置，法线，切线，一个纹理坐标
appdata_full |顶点着色器输入与位置，法线，切线，顶点颜色和两个纹理坐标
appdata_img |顶点着色器输入与坐标和一个纹理坐标

