---
title: adb常用命令
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

## 环境的配置

### 一、需要下载的文件：android-paltform-tools
> 下载链接：[android-paltform-tools][1]

![截图][2]

**根据系统，下载相应的版本。**

### 二、配置环境变量（windows）
1. 右击桌面的`此电脑（有些电脑叫计算机）`，选择`属性`

![属性对话框][3]

2. 选择左侧的`高级系统设置`，在弹出的对话框中选择右下角`环境变量`。

![环境变量窗口][4]

3. 在环境变量对话框中，找到`Path`属性，双击，

![编辑属性][5]

4. 在变量值输入框中，原有的内容后加上adb所在的目录，目录前用`;`隔开（英文分号）。`%ANDROID_HOME%`是paltform-tools所在的目录。

![修改参数][6]

## 常用命令

在使用adb命令前需要手机与电脑相连，并且手机进入开发者模式。android手机默认是不进入开发者模式的，具体操作可以自行百度。

使用adb命令必须进入终端:
- Windows：win+r，输入cmd，回车
- Linux：ctrl+alt+t
- Mac：command+空格，输入terminal，回车。

### 命令

- adb devices
	- 列举电脑连接的所有android设备
- adb -s 设备编号 命令
	- 电脑连接多个设备时，对指定设备进行操作 
- adb install apk路径+apk文件
	- 安装指定apk到设备
- adb uninstall apk包名
	- 卸载指定apk

> 安装apk软件之前，必须先卸载原有的apk，才能再次安装。如果不想这么麻烦，可以使用以下命令。
> **adb install -r apk路径+apk文件**

- adb pull 设备文件位置 本地存放位置
	- 将设备文件传至电脑中
- adb push 本地文件位置 设备存放位置
	- 将电脑文件传至设备中

> 这俩操作可以直接在电脑资源管理器中进行操作

- adb shell
	- 进入设备的终端

> 在设备的终端中，可以使用一些基本的linux命令,例如ps，ls，cd，top....退出该终端，直接输入exit回车即可。如果不想进入设备终端还想使用shell里的命令，可以直接**adb shell 命令**

如：

- adb shell am start 包名
	- 启动指定应用

### 测试类方法


 **adb shell monkey -v -p your.package.name 500**
 
- 参数`-p`：指定一个包名，即指定一个应用，指定多个包时可以`-p package1 –p package2  -p package3 `
- 最后的500为随机产生点击事件的次数
- 参数`-v`：指定日志级别，共三种级别
	- 一个`-v`：缺省值，仅提供启动提示、测试完成和最终结果等少量信息
	- 两个`-v`：提供较为详细的日志，包括每个发送到Activity的事件信息
	- 三个`-v`：最详细的日志，包括了测试中选中/未选中的Activity信息
- 参数`-s`：指定伪随机数生成器的seed值，如果seed相同，则两次Monkey测试所产生的点击事件的顺序是一样的
- 参数`--throttle <毫秒>`：指定点击事件产生的时间间隔
- 参数`--kill-process-after-error`：指定当应用程序发生错误时，是否停止其运行。如果指定此参数，当应用程序发生错误时，应用程序停止运行并保持在当前状态
- 参数`--pct-｛事件类别｝｛事件类别百分比｝`:指定每种类别事件的数目百分比

|  参数   |  解释   | 示例    |
| :---: | --- | :---: |
|  --pct-touch ｛百分比｝   |   手指按下屏幕事件的百分比  |  adb shell monkey -p com.mmednet.robot --pct-touch 10 1000   |
|  --pct-motion ｛百分比｝   |   调整动作事件的百分比(动作事件由屏幕上某处的一个down事件、一系列的伪随机事件和一个up事件组成)  |  adb shell monkey -p com.mmednet.robot --pct-motion 20 1000   |
|  --pct-trackball ｛百分比｝   |  调整轨迹事件的百分比(轨迹事件由一个或几个随机的移动组成，有时还伴随有点击)   |   adb shell monkey -p com.mmednet.robot --pct-trackball 30 1000  |
|  --pct-nav ｛百分比｝   |  调整“基本”导航事件的百分比(导航事件由来自方向输入设备的up/down/left/right组成)   |  adb shell monkey -p com.mmednet.robot --pct-nav 40 1000   |
|   --pct-majornav ｛百分比｝  |  调整“主要”导航事件的百分比(这些导航事件通常引发图形界面中的动作，如：5-way键盘的中间按键、回退按键、菜单按键)   |   adb shell monkey -p com.mmednet.robot --pct-majornav 50 1000  |
|  --pct-syskeys ｛百分比｝   |  调整“系统”按键事件的百分比(这些按键通常被保留，由系统使用，如Home、Back、Start Call、End Call及音量控制键)   |  adb shell monkey -p com.mmednet.robot --pct-syskeys 60 1000   |
|  --pct-appswitch ｛百分比｝   |  调整启动Activity的百分比。在随机间隔里，Monkey将执行一个startActivity()调用，作为最大程度覆盖包中全部Activity的一种方法   |  adb shell monkey -p com.mmednet.robot --pct-appswitch 70 1000   |
|  --pct-anyevent ｛百分比｝   |  调整其它类型事件的百分比。它包罗了所有其它类型的事件，如：按键、其它不常用的设备按钮、等等   |   adb shell monkey -p com.mmednet.robot --pct-anyevent 0 |
|     |   指定多个类型事件的百分比  |   adb shell monkey -p com.htc.Weather--pct-anyevent 50 --pct-appswitch 50 1000  |

> 注意：各事件类型的百分比总数不能超过100%

> adb还有很多命令，需要可以去自行百度
> 如：获取日志并保存至文件，电脑与设备间无线连接，截图，录屏，查看apk内存使用情况，cup使用情况，网速，设备的硬件信息，为文本框输入文字，模拟来电，模拟来短信等等等

  [1]: http://www.androiddevtools.cn/#sdk-platform-tools
  [2]: ./images/1514357148587.jpg
  [3]: ./images/1514357351698.jpg
  [4]: ./images/1514357547150.jpg
  [5]: ./images/1514357693084.jpg
  [6]: ./images/1514357875437.jpg