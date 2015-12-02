---
layout: post
title: Ubuntu 配置
category: 技术
tags:
keywords:
description:
---

虽然Linux不太会用，可是比较喜欢装系统。本文记录Ubuntu下自己用的配置和软件包，方便装系统后快速折腾。

## 虚拟机下安装
### open-vm-tools
在装VMware Tools时，官方推荐用`open-vm-tools`了，听官方的。
```
apt-get install open-vm-tools
#vm-tools安装方法。
```

没搞懂，我的系统是Ubuntu 14.04LTS，总共装了四个包才搞定,使用`dpkg -l|grep open-vm`查看安装的包不知道哪个是多余的。：
```
mindy@dog:~$ dpkg -l|grep open-vm
ii  open-vm-toolbox-lts-trusty  2:9.4.0-1280544-5ubuntu6.2  all Open VMware Tools for virtual machines hosted on VMware (transitional package)
ii  open-vm-tools-desktop2:9.4.0-1280544-5ubuntu6.2 amd64   Open VMware Tools for virtual machines hosted on VMware (CLI)
ii  open-vm-tools-desktop   2:9.4.0-1280544-5ubuntu6.2  amd64   Open VMware Tools for virtual machines hosted on VMware (GUI)
ii  open-vm-tools-lts-trusty    2:9.4.0-1280544-5ubuntu6.2  all Open VMware Tools for virtual machines hosted on VMware (transitional package)
```

# 显示语言
## 终端界面改为中文
在`/etc/environment`中加入
```
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh:en_US:en"
```
运行`sudo dpkg-reconfigure locales`,重启生效。

## 个人目录下文件夹名称改为英文
```
export LANG=en_US
xdg-user-dirs-gtk-update
```
据我观察`xdg-user-dirs-gtk-update`命令是在`~/.config/user-dirs.dirs`中写入了配置。

#搜狗输入法
以前用的ibus的谷歌拼音也不错，不过貌似也不怎么维护，上搜狗！

官网下载.deb包，双击安装，如果安装失败，添加软件源`ppa:fcitx-team/nightly`双击安装、更改输入法ibus为fcitx，重启即可。


杂项
>- tree
>- sl
>- zsh
>- https://github.com/robbyrussell/oh-my-zsh
>- vim
>- git
