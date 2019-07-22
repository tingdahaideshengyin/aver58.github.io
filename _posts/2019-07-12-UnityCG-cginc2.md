---
layout:     post
title:      "【详细】Unity中UnityCG.cginc中常用的函数"
subtitle:   "Shader入门"
date:       2019-07-12 17:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Shader
---

# Unity中UnityCG.cginc中常用的函数(详解)



## 摄像机方向
### WorldSpaceViewDir
> 输入一个模型空间中的顶点坐标    ==》输出（世界空间）从这个点到摄像机的观察方向；
```
// 内部实现也是用UnityWorldSpaceViewDir
// Computes world space view direction, from object space position
// *Legacy* Please use UnityWorldSpaceViewDir instead
inline float3 WorldSpaceViewDir( in float4 localPos )
{
    float3 worldPos = mul(unity_ObjectToWorld, localPos).xyz;
    return UnityWorldSpaceViewDir(worldPos);
}
```
### UnityWorldSpaceViewDir
> 输入一个模型空间中的顶点坐标    ==》输出（世界空间）从这个点到摄像机的观察方向；
```
// Computes world space view direction, from object space position
inline float3 UnityWorldSpaceViewDir( in float3 worldPos )
{
    return _WorldSpaceCameraPos.xyz - worldPos;
}
```
### ObjSpaceViewDir
> 模型空间中的顶点坐标            ==》模型空间从这个点到摄像机的观察方向；
```
// Computes object space view direction
inline float3 ObjSpaceViewDir( in float4 v )
{
    float3 objSpaceCameraPos = mul(unity_WorldToObject, float4(_WorldSpaceCameraPos.xyz, 1)).xyz;
    return objSpaceCameraPos - v.xyz;
}
```

## 光源方向
### WorldSpaceLightDir
> 模型空间中的顶点坐标  ==》世界空间中从这个点到光源的方向
```
// Computes world space light direction, from object space position
// *Legacy* Please use UnityWorldSpaceLightDir instead
inline float3 WorldSpaceLightDir( in float4 localPos )
{
    float3 worldPos = mul(unity_ObjectToWorld, localPos).xyz;
    return UnityWorldSpaceLightDir(worldPos);
}
```
### UnityWorldSpaceLightDir
> (ForwardBase Only,not normalized)世界空间中的顶点坐标 ==》世界空间中从这个点到光源的方向
```
// Computes world space light direction, from world space position
inline float3 UnityWorldSpaceLightDir( in float3 worldPos )
{
    #ifndef USING_LIGHT_MULTI_COMPILE
        return _WorldSpaceLightPos0.xyz - worldPos * _WorldSpaceLightPos0.w;
    #else
        #ifndef USING_DIRECTIONAL_LIGHT
        return _WorldSpaceLightPos0.xyz - worldPos;
        #else
        return _WorldSpaceLightPos0.xyz;
        #endif
    #endif
}
```
### ObjSpaceLightDir
> (ForwardBase Only,not normalized)模型空间中的顶点坐标    ==》模型空间中从这个点到光源的方向；
```
// Computes object space light direction
inline float3 ObjSpaceLightDir( in float4 v )
{
    float3 objSpaceLightPos = mul(unity_WorldToObject, _WorldSpaceLightPos0).xyz;
    #ifndef USING_LIGHT_MULTI_COMPILE
        return objSpaceLightPos.xyz - v.xyz * _WorldSpaceLightPos0.w;
    #else
        #ifndef USING_DIRECTIONAL_LIGHT
        return objSpaceLightPos.xyz - v.xyz;
        #else
        return objSpaceLightPos.xyz;
        #endif
    #endif
}
```

## 方向转换

### UnityObjectToWorldNormal
> 法线方向矢量 模型空间==》世界空间
```
// Transforms normal from object to world space
inline float3 UnityObjectToWorldNormal( in float3 norm )
{
#ifdef UNITY_ASSUME_UNIFORM_SCALING
    return UnityObjectToWorldDir(norm);
#else
    // mul(IT_M, norm) => mul(norm, I_M) => {dot(norm, I_M.col0), dot(norm, I_M.col1), dot(norm, I_M.col2)}
    return normalize(mul(norm, (float3x3)unity_WorldToObject));
#endif
}
```
### UnityObjectToWorldDir
> 方向矢量 模型空间==》世界空间
```
// Transforms direction from object to world space
inline float3 UnityObjectToWorldDir( in float3 dir )
{
    return normalize(mul((float3x3)unity_ObjectToWorld, dir));
}
```
### UnityWorldToObjectDir
> 方向矢量 世界空间==》模型空间
```
// Transforms direction from world to object space
inline float3 UnityWorldToObjectDir( in float3 dir )
{
    return normalize(mul((float3x3)unity_WorldToObject, dir));
}
```
### TANGENT_SPACE_ROTATION
> 模型空间==》切线空间
```
// Declares 3x3 matrix 'rotation', filled with tangent space basis
#define TANGENT_SPACE_ROTATION \
    float3 binormal = cross( normalize(v.normal), normalize(v.tangent.xyz) ) * v.tangent.w; \
    float3x3 rotation = float3x3( v.tangent.xyz, binormal, v.normal )

```

## 顶点计算
### TRANSFORM_TEX
> 缩放和偏移2D纹理的UV，对纹理坐标进行变换，对应材质面板的Tiling和Offset调节项
```
// Transforms 2D UV by scale/bias property
#define TRANSFORM_TEX(tex,name) (tex.xy * name##_ST.xy + name##_ST.zw)
```

### UnpackNormal
```
// Unpack normal as DXT5nm (1, y, 1, x) or BC5 (x, y, 0, 1)
// Note neutral texture like "bump" is (0, 0, 1, 1) to work with both plain RGB normal and DXT5nm/BC5
fixed3 UnpackNormalmapRGorAG(fixed4 packednormal)
{
    // This do the trick
   packednormal.x *= packednormal.w;

    fixed3 normal;
    normal.xy = packednormal.xy * 2 - 1;
    normal.z = sqrt(1 - saturate(dot(normal.xy, normal.xy)));
    return normal;
}
inline fixed3 UnpackNormal(fixed4 packednormal)
{
#if defined(UNITY_NO_DXT5nm)
    return packednormal.xyz * 2 - 1;
#else
    return UnpackNormalmapRGorAG(packednormal);
#endif
}
```

### clip
```
	// Alpha test
	clip (texColor.a - _Cutoff);
	// Equal to 
    // if ((texColor.a - _Cutoff) < 0.0) {
    // 	    discard;
    // }
```





## Data结构在UnityCG.cginc
