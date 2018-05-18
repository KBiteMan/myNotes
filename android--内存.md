---
title: android--内存
tags: 内存
grammar_cjkRuby: true
---

单个App应用是有最大内存限制的，其大小，与机器有关，可以通过以下方式查看：
 方法一：
 adb shell getprop | grep dalvik.vm.heapgrowthlimit
 方法二：
```java
ActivityManager activityManager =(ActivityManager)context.getSystemService(Context.ACTIVITY_SERVICE);
activityManager.getMemoryClass();
activityManager.getLargeMemoryClass();
```
方法三：
adb shell cat /system/build.prop | grep heap
方法四：
Runtime.getRuntime().maxMemory()

## 为什么要有内存限制：
android的应用时基于java的，并且每个应用的jvm是独立的，这种独立可以避免应用崩溃影响整个系统。
JVM的内存管理是基于预分配的（叫“堆”），重分配堆内存效率很低，所以就有了初始堆内存大小和最大堆内存大小相关的参数。