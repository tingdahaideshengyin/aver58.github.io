---
published: false
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
