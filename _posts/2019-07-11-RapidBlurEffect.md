---
layout:     post
title:      "【Unity Shaders】RapidBlurEffect —— 快速模糊"
subtitle:   "Shader入门"
date:       2019-07-10 11:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Shader
---

> 学习完UnityShader入门精要的前5章，总算开始动手写shader，但是凭空让我写，我咋知道写什么呢。
还是看看项目的Shader，学习一下吧！

#####项目最常用的Shader就是ui界面的背景模糊:
```
Shader "_Aver3/20190710FastBlur"
{
	Properties
	{
		_MainTex("Base (RGB)", 2D) = "white" {}
		_blurSize ("Blur Size", Range(0,10)) = 1.0
	}
	SubShader
	{
	    // 透明队列，在所有不透明的几何图形后绘制
		Tags { "RenderType"="Transparent" }
		LOD 100

		Pass
		{
			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			// make fog work
			#pragma multi_compile_fog
			#pragma target 3.0

			#include "UnityCG.cginc"

			float _blurSize;
			sampler2D _MainTex;
			float4 _MainTex_ST;
			float4 _MainTex_TexelSize;

			struct a2v{
				float2 uv : TEXCOORD0;
				float4 vertex : POSITION;
			};

			struct v2f{
				float4 pos : SV_POSITION;
				float2 uv : TEXCOORD0;
				float2 offsetuv[4] : TEXCOORD1;
			};

			v2f vert(a2v IN){
				v2f o;
				o.pos = UnityObjectToClipPos(IN.vertex);
				// 处理UV的till和offset
				o.uv = TRANSFORM_TEX(IN.uv, _MainTex);

				// 为啥这个常量放在顶点函数外面效果差这么多？？？todo
				float2 offsetWeight[4] = {
					float2(0, 1),
					float2(-1, 0),
					float2(1, 0),
					float2(0, -1)
				};
				// 取周围4个像素做模糊
				o.offsetuv[0] = o.uv.xy + float2(offsetWeight[0].x, offsetWeight[0].y) * _MainTex_TexelSize * _blurSize;
				o.offsetuv[1] = o.uv.xy + float2(offsetWeight[1].x, offsetWeight[1].y) * _MainTex_TexelSize * _blurSize;
				o.offsetuv[2] = o.uv.xy + float2(offsetWeight[2].x, offsetWeight[2].y) * _MainTex_TexelSize * _blurSize;
				o.offsetuv[3] = o.uv.xy + float2(offsetWeight[3].x, offsetWeight[3].y) * _MainTex_TexelSize * _blurSize;

				return o;
			}
			
			fixed4 frag (v2f IN) : SV_Target
			{
				fixed4 col = fixed4(0, 0, 0, 0);
				// 取样4个像素，然后*0.25 得出平均值
				col += tex2D(_MainTex, IN.offsetuv[0]);
				col += tex2D(_MainTex, IN.offsetuv[1]);
				col += tex2D(_MainTex, IN.offsetuv[2]);
				col += tex2D(_MainTex, IN.offsetuv[3]);
				col *= 0.25;
				col.a = 1;
				// sample the texture
				// fixed4 col = tex2D(_MainTex, i.uv);

				// apply fog
				UNITY_APPLY_FOG(i.fogCoord, col);
				return col;
			}
			ENDCG
		}
	}
}
```
- 看了一下，就是取样周围4个像素做平均值模糊，这样的话，当模糊值大的时候，效果就很差。然后看了一下高斯模糊，学习了一下卷积的定义。后面再来填坑，搬砖搬砖
- 然后调用的地方，有点没看懂：
```C#
	void BlurRender(RenderTexture sourceTexture)
	{
		int rtW = sourceTexture.width;
		int rtH = sourceTexture.height;
		RenderTexture temporary = RenderTexture.GetTemporary(rtW, rtH, 0, sourceTexture.format);
		// 为啥要调用2次
		temporary.filterMode = FilterMode.Bilinear;
		temporary.Create();
		temporary.filterMode = FilterMode.Bilinear;

		// float widthMod = 1.0f / (1.0f * (1 << DownSampleNum));
		for (int i = 0; i < 2; i++)
		{
			float iterationOffs = i * 1.0f;
			BlurMaterial.SetFloat("_blurSize", BlurSize + iterationOffs);
			//垂直模糊
			temporary.DiscardContents();
			// 为啥要调用2次
			Graphics.Blit(sourceTexture, temporary, BlurMaterial, 0);
			sourceTexture.DiscardContents();
			Graphics.Blit(temporary, sourceTexture, BlurMaterial, 0);
		}
		RenderTexture.ReleaseTemporary(temporary);
		renderBuffer = sourceTexture;
		m_rawImage.texture = renderBuffer;
	}
```
