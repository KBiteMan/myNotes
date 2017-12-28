---
title: JavaScript入坑指南 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 一、变量
变量这东东和java的差不多,生命周期也一样,不同之处在于向没有使用var声明的变量无论是在函数还是在哪声明的变量,都会成为全局变量
## 二、数据类型
**字符串、数字、布尔、数组、对象、Null、Undefined**

### String（字符串）
引号中的任意文本,可以是单引号或者是双引号
### Number（数字）
只有一种数字类型,可以带小数点也可以不带小数点.也可以使用科学计数法表示。
和java一样，js也有进制这一说法。如：

```javascript
var intNum = 55;        //整数
var octalNum1 = 034;    //有效八进制
var octalNum2 = 089；   //无效八进制
var hexNum1 = 0xA;      //十六进制
```

> 在严格模式下，字面值八进制无效，使用时会报错。在算数运算时，和java一样，均会转换成十进制。

#### 1.浮点数值
必须包含小数点以及小数点后必须至少有以为数字。浮点数值在内存占用的空间是整数数值的两倍，所以js会不失时机的将浮点数值转换为整数值，如`10.0 , 1.`均会被转为整数。
浮点数值的最高精度为17位小数，但精确度不如整数，这是很多编程语言的通病

#### 2.数值范围
最小`Number.MIN_VALUE`——大多数浏览器这个值为5e-324。最大`Number.MAX_VALUE`——大多数浏览器这个值为1.7976313486231573+308。如果超出范围，则用Infinity表示，正无穷——`Infinity`，负无穷——`-Infinity`。Infinity不能参与运算。可以使用`isFinite()`函数进行判断是不是有穷的，只有在最大和最小范围内才会返回`true`

#### 3.NaN
即非数值，是一个特殊值，用于表示一个本来要返回数值的操作数为返回数值的情况。
有两个特点：

- 任何涉及NaN的操作都会返回NaN
- 与任何值都不相等，包括自己

可以使用`isNaN()`函数判断是否“不是数值”。他接受一个参数时会尝试将其转换为数值。所以任何不能转换为数值的值都会发挥`true`。

#### 4.数值转换

### Boolean（布尔）
和其他语言一样,只有两个值---true/false。但是在JavaScript中所用类型的值都可以转化为Boolean类型。直接使用`Boolean()`即可实现。
转换规则如下：
| 数据类型 |转换为true值|转换为false值|
| :----: | :---- | :---- |
|Boolean|true|false|
|String|任何非空字符串|""(空字符串)|
|Number|任何非零数字值（包括无穷大）|0和NaN|
|Object|任何对象|null|
|Undefined|不适用|undefined|
### 数组
有多种形式

 - var arr = new Array(); arr[0] = 1; arr[1] = 2;
 - var arr = new Array(1,2);
 - var arr = [1,2];

### Object（对象）
对象由花括号分隔.在括号内部属性以键值对的形式(name:value)来定义,用逗号隔开,对象的其他声明方法后面会有详细介绍


 - var person = {name:'Poul',age:'25',sex:'非女'};
 - 也可以分行书写
 - 寻址方式也有两种
  - userName = person.name;
  - userName = person["name"];
    
### Null和Undefined
- Undefined:表示变量不含有值,是一个没有设置值的bianl
- Null:可以通过将变量的值设置为null来清空变量,表示什么都没有

声明变量类型
声明新变量时,可以使用关键字"new"来声明类型

```javascript
var string = new String; 
var num = new Number; 
var bool = new Boolean; 
var arr = new Array; 
var person = new Object; 
```

## 三、输出语句

1. 使用window.alert()弹出警告对话框
2. 使用document.write()方法将内容写到html文档中
3. 使用innerHTML写到html元素
4. 使用console.log()写到浏览器控制台

## 四、注释

1. 单行注释://
2. 多行注释:/*......*/

## 五、运算符
运算符也是和java的一样的什么加减乘除,什么自增自减啥的,都一样
但是,在逻辑运算符中有两个java时没有的

- ===:绝对等于
- !==:绝对不等于
他们和==等于以及!=不等于的区别在于,===是值和类型都相等时才返回true,而==只要值相等,类型不一样也会返回true.

## 六、分支语句

1. if...else...语句:和java一样,变种也一样
2. switch语句:和java一样
3. for语句:除了foreach语句和java不同,也是一样的,它的foreach语句是for(a in b),还要注意的是他的foreach返回的不是遍历每项,而是遍历每项的下标.
4. while语句:和java一样

### break和continue
和java一样,break为跳出循环,continue为跳出本次循环并执行下次循环
### javascript标签
`label: lableName;`:使用的话直接将labelName跟随在break或continue后跟随即可.

## typeof与constructor的使用
typeof是用来检测数据类型的,如

```javascript
 typeof "John"                // 返回 string  
 typeof 3.14                  // 返回 number
 typeof false                 // 返回 boolean 
 typeof [1,2,3,4]             // 返回 object 
 typeof {name:'John', age:34} // 返回 object 
```

>Null数据类型为object,什么都没有,表示一个对想的空引用
Undefined数据类型为undefined

constructor返回的是所有javascript变量的构造函数
typeof对数组,日期类等返回的都是object,但是constructor返回的则是相应的类型.

```javascript
"你好".constructor;         //返回[Function: String]
```

## 七、数据类型转换

- 数字转为字符串,将布尔转为字符串,将日期转为字符串
    - 全局方法String(),可用于任何类型的数字,字母,变量,表达式
    - Number方法toString(),同样效果
- 将字符串转为数字,将布尔值转为数字,将日期转为数字
    - 全局方法 Number() 可以将字符串转换为数字.字符串包含数字(如 "3.14") 转换为数字 (如 3.14).空字符串转换为 0。其他的字符串会转换为 NaN (不是个数字)
- 一元运算符`+`
    - 可以将变量转为数字,如:

```javascript
var y = "5";      // y 是一个字符串 
var x = + y;     // x 是一个数字 
```

- 自动转换数据类型

```javascript
5 + null    // 返回 5         null 转换为 0 
"5" + null  // 返回"5null"   null 转换为 "null" 
"5" + 1     // 返回 "51"      1 转换为 "1"   
"5" - 1     // 返回 4         "5" 转换为 5 
```

## 八、函数
### 函数的基本语法

```javascript
function functionName(var1,var2,...){//可以有参数,也可以没 
   //需要执行的代码 
}
```

带返回值的函数
和java的方法一样,可以直接return回来
## 九、对象
和java的对象类似,也是有属性和方法的,使用时也和java类似
### 对象的定义
比较流行的定义方法混合的构造函数/原型方式

```javascript
 function Car(sColor,iDoors,iMpg) { 
   this.color = sColor; 

   this.doors = iDoors; 

   this.mpg = iMpg; 

   this.drivers = new Array("Mike","John"); 
 } 
  
 Car.prototype.showColor = function() { 
   alert(this.color); 
 }; 
```

## 十、错误
和java类似,也是使用try...catch(err)...语句处理错误.也是有throw语句的,使用也是很简单,直接在需要的地方throw exception即可,其中的exception可以是字符串,数字,逻辑值或对象

## 十一、严格模式
1. 不允许使用未声明的变量,对象也是一个变量。
2. 不允许删除变量或对象。
3. 不允许删除函数。
4. 不允许变量重名
5. 不允许使用八进制
6. 不允许使用转义字符
7. 不允许对只读属性赋值
8. 不允许对一个使用getter方法读取的属性进行赋值
9. 不允许删除一个不允许删除的属性
10. 变量名不能使用 "eval" 字符串
11. 变量名不能使用 "arguments" 字符串
12. 不允许使用以下这种语句:
```javascript
"use strict"; 
with (Math){x = cos(2)}; // 报错 
```
13.由于一些安全原因，在作用域 eval() 创建的变量不能被调用
14.禁止this关键字指向全局对象