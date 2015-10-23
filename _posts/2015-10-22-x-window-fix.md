---
layout: post
title: Linux不能进入x window
category: 技术
tags:
keywords: 
description:
---
老师给我分配一个GPU工作站，刚开机好像我把`/etc/X11/xorg.conf`搞坏了，导致不能进入x window, 可把我急坏了，按照[这里][1]的步骤做下来就好，发现自己太菜了，记录下来。。

解决方法就是重新生成一个`/etc/X11/xorg.conf`文件

### 生成新的`/etc/X11/xorg.conf`
第一步是以超级用户的身份建立初始的配置文件：
```
# Xorg -configure
```
下一步是测试现存的配置文件(不建议用这个，会黑屏，不知道情况的会抓狂)
```
# Xorg -config xorg.conf.new
```
通过 retro 选项使用旧的模式：
```
# Xorg -config xorg.conf.new -retro
```
使用组合键`Ctrl+Alt+Fn`回到终端.
最后一步将新的`xorg.conf`放到他该在的位置。
```
# cp xorg.conf.new /etc/X11/xorg.conf
```

如此简单

[1]:https://www.freebsd.org/doc/zh_CN.UTF-8/books/handbook/x-config.html "配置 X11"
