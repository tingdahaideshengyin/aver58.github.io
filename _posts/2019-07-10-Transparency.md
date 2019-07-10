---
layout:     post
title:      "【Unity Shaders】Transparency —— 使用渲染队列进行深度排序"
subtitle:   "Shader入门"
date:       2019-07-10 11:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Shader
---

# 【Unity Shaders】Transparency —— 使用渲染队列进行深度排序

<div class="table-box"><table border="1" width="800" cellspacing="1" cellpadding="1"><tbody><tr><td>渲染队列</td><td>渲染队列描述</td><td>渲染队列值</td></tr><tr><td>Background</td><td>这个队列通常被最先渲染。</td><td>1000</td></tr><tr><td>Geometry</td><td>这是默认的渲染队列。它被用于绝大多数对象。不透明几何体使用该队列。</td><td>2000</td></tr><tr><td>AlphaTest</td><td>通道检查的几何体使用该队列。它和Geometry队列不同，对于在所有立体物体绘制后渲染的通道检查的对象，它更有效。</td><td>2450</td></tr><tr><td>Transparent</td><td>该渲染队列在Geometry和AlphaTest队列后被渲染。任何通道混合的（也就是说，那些不写入深度缓存的Shaders）对象使用该队列，例如玻璃和粒子效果。</td><td>3000</td></tr><tr><td>Overlay</td><td>该渲染队列是为覆盖物效果服务的。任何最后被渲染的对象使用该队列，例如镜头光晕。</td><td>4000</td></tr></tbody></table></div>
1. 最后，我们还要在SubShader块中声明ZWrite标签。这告诉Unity，我们想要重写对象的深度排序，并且我们将会为它指定一个新的渲染队列。因此，我们就简单的把ZWrite值设为Off。（不设就没有效果）
