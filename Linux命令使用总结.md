---
title: Linux命令使用总结
tags: linux
grammar_cjkRuby: true
---

## 时间
- 查看时间：`date`
- 查看时间和时区：`date -R`
- 查看日历：`cal`
### 设置时区：
- 通用，`tzselect`，我遇到过无效果的时候
- RedHat Linux 和 CentOS，`timeconfig`
- Debian，`dpkg-reconfigure tzdata`
-  复制相应的时区文件，替换系统时区文件;或者创建链接文件
	-  cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
### 设置时间：
- `date -s 11/03/2009`：将系统日期设定成2009年11月3日
- `date -s 17:55:55`：将系统时间设定成下午5点55分55秒
- 将当前时间和日期写入BIOS，避免重启后失效：`hwclock -w`

