---
layout:     post
title:      "【详细】UnityShaderUtilities.cginc中常用的函数"
subtitle:   "Selenium入门"
date:       2019-07-20 18:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Shader
---

# 【详细】UnityShaderUtilities.cginc中常用的函数

### UnityObjectToClipPos 
> 输入一个模型空间中的顶点坐标    ==》输出（裁剪空间）坐标；
```
inline float4 UnityObjectToClipPosODS(float3 inPos)
{
    float4 clipPos;
    float3 posWorld = mul(unity_ObjectToWorld, float4(inPos, 1.0)).xyz;
#if defined(STEREO_CUBEMAP_RENDER_ON)
    float3 offset = ODSOffset(posWorld, unity_HalfStereoSeparation.x);
    clipPos = mul(UNITY_MATRIX_VP, float4(posWorld + offset, 1.0));
#else
    clipPos = mul(UNITY_MATRIX_VP, float4(posWorld, 1.0));
#endif
    return clipPos;
}

// Tranforms position from object to homogenous space
inline float4 UnityObjectToClipPos(in float3 pos)
{
#if defined(STEREO_CUBEMAP_RENDER_ON)
    return UnityObjectToClipPosODS(pos);
#else
    // More efficient than computing M*VP matrix product
    return mul(UNITY_MATRIX_VP, mul(unity_ObjectToWorld, float4(pos, 1.0)));
#endif
}

inline float4 UnityObjectToClipPos(float4 pos) // overload for float4; avoids "implicit truncation" warning for existing shaders
{
    return UnityObjectToClipPos(pos.xyz);
}
```