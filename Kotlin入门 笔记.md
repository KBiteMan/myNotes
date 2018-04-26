---
title: Kotlin入门 笔记
tags: Kotlin
grammar_cjkRuby: true
---

## 1.常量
可变变量定义
```kotlin
var <标识符>:<变量类型> = <初始化值>
```
不可变变量定义
```kotlin
val <标识符>:<变量类型> = <初始化值>
```

## 2.函数

```kotlin
fun add(a:Int,b:Int):Int{
	return a+b;
}
```
格式：fun 函数名（参数名：参数类型）：返回值类型{}

### 可变长参数函数

```kotlin
fun vars(vararg v:Int){
    for(vt in v){
        print(vt)
    }
}
```

## 3.类

```kotlin
class Demo(name:String){
	fun demo(){
		println("你好")
	}
}
```
