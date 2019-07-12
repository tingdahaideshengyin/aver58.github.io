---
layout:     post
title:      "【设计模式与游戏完美开发】单例模式简析"
subtitle:   "设计模式"
date:       2019-07-10 16:00:00
author:     "Zero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Coder's Life
---

> 我是设计转行的，有一次被面试到实现个单例，好在之前大佬有写过，当时自己也没在意，蛮看了一下就过了，真正要用的时候，肚子真的空空如也，而且是手写，对于我这种离开VS studio就不会写代码的人，简直就是折磨。凭着记忆大概写出来了，但是面试官一看，你这个连public都没写，别人怎么实例化，而且if条件语句竟然用的是=，而不是==。当时的我就···，面试自然也就不了了之。所以单例还是很重要的，在这里复习一下。

    /// <summary>
    /// 最基础的单例，但是只能在单线程中使用
    /// </summary>
    public sealed class Singleton0
    {
        public Singleton0() { }

        private static Singleton0 m_instance;
        public Singleton0 Instance {
            get {
                if (m_instance == null)
                    m_instance = new Singleton0();
                return m_instance;
            }
        }
    }

    /// <summary>
    /// 多线程中可使用的单例，但是同步锁比较耗时
    /// </summary>
    public sealed class SingletonMuliti
    {
        public SingletonMuliti() { }

        private static SingletonMuliti m_instance;
        // 设置一个同步锁
        private static readonly object syncObj;
        public SingletonMuliti Instance
        {
            get
            {
                //加锁非常耗时
                lock (syncObj)
                {
                    if (m_instance == null)
                        m_instance = new SingletonMuliti();
                }
                return m_instance;
            }
        }
    }


    /// <summary>
    /// 改进后的多线程同步锁，在加锁前后都判断一下空
    /// </summary>
    public sealed class SingletonMuliti2
    {
        public SingletonMuliti2() { }

        private static SingletonMuliti2 m_instance;
        private static readonly object syncObj;
        public SingletonMuliti2 Instance
        {
            get
            {
                //加锁前判断实例是否存在
                if (m_instance == null)
                {
                    lock (syncObj)
                    {
                        if (m_instance == null)
                            m_instance = new SingletonMuliti2();
                    }
                }
                return m_instance;
            }
        }
    }

    /// <summary>
    /// 静态构造单例，但可能过早创建
    /// </summary>
    public sealed class SingletonStatic
    {
        public SingletonStatic() { }
        //在初始化静态变量的时候创建一个实例。而且静态构造函数只会调用一次。
        //但是静态构造函数不受程序员控制，它是在CLR第一次使用这个类型时候自动构造的。
        //这样可能实例会过早地创建，影响效率
        private static SingletonStatic m_instance = new SingletonStatic();
        public SingletonStatic Instance
        {
            get
            {
                return m_instance;
            }
        }
    }

    /// <summary>
    /// 按需静态构造单例，嵌套构造函数
    /// </summary>
    public sealed class SingletonStaticInNeed
    {
        public SingletonStaticInNeed() { }
        public SingletonStaticInNeed Instance
        {
            get
            {
                return Nest.instance;
            }
        }

        private class Nest
        {
            static internal SingletonStaticInNeed instance = new SingletonStaticInNeed();
        }
    }
跟着书上的案例写了上面几个，然后赶紧看看项目中的单例，还是项目写的靠谱


    /// <summary>
    /// 干净的写法，项目中用的
    /// </summary>
    public sealed class Singleton
    {
        public Singleton() { }
        private static Singleton m_instance;
        public Singleton Instance
        {
            get
            {
                return m_instance ?? (m_instance = new Singleton());
            }
        }
    }
然后这章结束的时候还留了个小题，自己蛮做了一下。
    //要求定义一个表示总体的类President，
    //可以从这个类继承出FrenchPresident和AmericanPresident等类型，
    //这些派生类都只能产生一个实例。


    public class President
    {
        public President() { }
        private static President m_instance;
        public President Instance
        {
            get
            {
                return m_instance ?? (m_instance = new President());
            }
        }
    }

    public sealed class FrenchPresident : President
    {
       
    }

    public sealed class AmericanPresident : President
    {

    }
实现原理在于，

    public 修饰符，保证它们能被访问到
    sealed 修饰符，保证它们不能作为其他类型的基类