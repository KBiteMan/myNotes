---
title: Android——Activity
tags: 
grammar_cjkRuby: true
---

## 一、生命周期

![生命周期][1]

### onCreate方法
Activity第一次创建的时候调用。这个方法里主要是提供给我们做一些初始化操作，如：创建view、绑定数据到view。同时这个方法里还带有一个Bundle参数。

### onStart方法
此时Activity已经显示，但是用户还不能进行操作

### onResume方法
当前情况下，Activity可与用户进行交互。

### onPause方法
系统准备停止当前activity被调用，建议在此处持久化一些数据和停止动画等，不宜做耗时操作

### onStop方法
activity已经不再显示给用户，并且新的activity在此之前onStart或者onResume，此时可以做一些重量级的回收操作

### onRestart方法
activity重新启动，接下来将继续走到onStart和onResume

### onDestroy方法
activity被销毁时会被调用，可做一些回收操作。activity被销毁，但是static全局变量还没有被销毁，activity结束不代表app结束。因此当activity再次启动时，可能会产生脏数据（dirty data）

## 二、与生命周期相关的方法
## onSaveInstanceState方法和onRestoreInstanceState

## onConfigurationChanged方法

## onNewIntent方法

  [1]: ./images/1525930434708.jpg