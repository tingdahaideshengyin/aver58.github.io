---
layout:     post
title:      "协程简析"
subtitle:   "C#入门"
date:       2019-07-20 18:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Unity
---

# 协程简析
```
   IEnumerator StartTest()
    {
        for(int i = 0; i < 100; i++)
        {
            print(i.ToString());
        }
        yield return null;
    }
    输出：0,0,0,0,0,0,0,0,0,0……
```

```
   IEnumerator StartTest()
    {
        for(int i = 0; i < 100; i++)
        {
            print(i.ToString());
            yield return null;
        }
    }
    输出：0,1,2,3,4,5,6,7,8,9……
```

> yield return null :直接 call ==> MoveNext()

我的理解是直接调用迭代器实例的下一次