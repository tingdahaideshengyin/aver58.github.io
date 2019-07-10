---
published: true
---
---
layout:     post
title:      "Unity中UnityCG.cginc中常用的函数"
subtitle:   "Shader入门"
date:       2019-07-09 15:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Shader
---


# Unity中UnityCG.cginc中常用的函数


### 摄像机方向（视角方向）
1. float3 WorldSpaceViewDir ( float4 v )      	//根据模型空间中的顶点坐标==》（世界空间）从这个点到摄像机的观察方向；
2. float3 UnityWorldSpaceViewDir ( float4 v )    //世界空间的顶点坐标==》世界空间从这个点到摄像机的观察方向；
3. float3 ObjSpaceViewDir (float4 v )     		//模型空间中的顶点坐标==》模型空间从这个点到摄像机的观察发方向；

### 光源方向
1. float3 WorldSpaceLightDir ( float4 v )    		//模型空间中的顶点坐标==》世界空间中从这个点到光源的方向；
2. float3 UnityWorldSpaceLightDir ( float4 v )     //世界空间中的顶点坐标==》世界空间中从这个点到光源的方向；
3. float3 ObjSpaceLightDir ( float4 v )    			//模型空间中的顶点坐标==》模型空间中从这个点到光源的方向；

### 方向转换
1. float3 UnityObjectToWorldNormal ( float3 norm )    //把法线方向 模型空间==》世界空间；
2. float3 UnityObjectToWorldDir ( float3 dir )      //把方向 模型空间==》世界 空间；
3. float3 UnityWorldToObjectDir ( float3 dir )     //把方向 世界空间==》模型空间；


### Data结构在UnityCG.cginc　
appdata_base：顶点着色器输入与位置,法线,纹理坐标
appdata_tan：顶点着色器输入与位置，法线，切线，一个纹理坐标
appdata_full：顶点着色器输入与位置，法线，切线，顶点颜色和两个纹理坐标
appdata_img：顶点着色器输入与坐标和一个纹理坐标
