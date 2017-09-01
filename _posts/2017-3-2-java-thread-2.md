---
title: 深入理解Java多线程二（Java线程的默认值）
date: 2017-3-2 22:30:09
categories:
- Java
---

我想通过[上一篇文章](../java-thread-1)不可能让你彻底全面掌握OS层面的多线程技术，但是我们需要对以下内容有一定认识：  

- 线程是进程中的一个控制点而已
- 线程是在CPU核的硬件线程上被运行的
- 每个线程有自己的内存堆栈

----

**开始进入Java多线程技术的学习吧！！**


# Java Thread类源码

## 阅读源码

```java
public class Thread implements Runnable {
    /* Make sure registerNatives is the first thing <clinit> does. */
    private static native void registerNatives();
    static {
        registerNatives();
    }

    private volatile String name;
    private int            priority;
    /* Whether or not the thread is a daemon thread. */
    private boolean     daemon = false;
    /* What will be run. */
    private Runnable target;


    /**
     * The minimum priority that a thread can have.
     */
    public final static int MIN_PRIORITY = 1;

   /**
     * The default priority that is assigned to a thread.
     */
    public final static int NORM_PRIORITY = 5;

    /**
     * The maximum priority that a thread can have.
     */
    public final static int MAX_PRIORITY = 10;

```

## 几个小问题

## 深入学习


# 创建自己的Thread
