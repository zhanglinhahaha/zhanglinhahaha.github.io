---
layout: post
title: 【Android】LeakCanary 的使用
date: 2021-01-27
description: LeakCanary 继承到项目，并分析其错误。
tag: Android
---

## LeakCanary 简介

LeakCanary 是用来检测内存泄漏的一个库。它能够帮助开发人员更加准确快速的定位内存泄露的地方。

[引用1，LeakCanary 官方文档](https://square.github.io/leakcanary/fundamentals-fixing-a-memory-leak/)

## 使用方法
LeakCanary 的使用方法也很简单，只需要在 Android Studio 项目的 build.gradle 文件中添加依赖就可以，如下：

```java
dependencies {
    // debugImplementation because LeakCanary should only run in debug builds.
    debugImplementation 'com.squareup.leakcanary:leakcanary-android:2.6'
}
```

然后编译项目，推进系统，用 logcat 抓取 TAG 类型为 LeakCanary 的 log，如果能够抓取到相关的信息，则说明已经成功了。

那么要如何测试呢？

如果你对项目的每一个功能都了如指掌，或者有这个项目的 featurelist，那么你可以把其全部功能测一遍，把 log 存下来，后面分析会用到。

那么你如果想偷懒，或者说是不是很了解其全部功能，那么 monkey 是再适合不过了。

[引用2， monkey的相关介绍](https://www.jianshu.com/p/c8844327f5e9)

## LeakCanary log 分析

借用官方的例子，eg.

```
┬───
│ GC Root: System class
│
├─ android.provider.FontsContract class
│    ↓ static FontsContract.sContext
├─ com.example.leakcanary.ExampleApplication instance
│    Leaking: NO (Application is a singleton)
│    ↓ ExampleApplication.leakedViews
│                         ~~~~~~~~~~~
├─ java.util.ArrayList instance
│    ↓ ArrayList.elementData
│                ~~~~~~~~~~~
├─ java.lang.Object[] array
│    ↓ Object[].[0]
│               ~~~
├─ android.widget.TextView instance
│    Leaking: YES (View.mContext references a destroyed activity)
│    ↓ TextView.mContext
╰→ com.example.leakcanary.MainActivity instance
```

上面的输出表示项目发生了内存泄露，并打印了其内存泄漏堆栈。

首先需要找到不是 Leaking: YES 的一行，表示内存泄漏发生在这里，然后根据提示，可以找到内存泄漏的对象。

比如上面，发生内存泄露的对象就是 TextView.mContext。

至于要怎么避免内存泄漏，这个问题应该是八仙过海了~
