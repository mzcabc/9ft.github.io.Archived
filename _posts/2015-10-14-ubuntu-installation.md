---
layout: post
title: Ubuntu 配置
date: 2015-10-14 00:00:00 +0800
update_date: 2016-08-25 14:09:29 +0800
category: 技术
---
虽然 `Linux` 不太会用, 可是比较喜欢装系统. 本文记录 `Ubuntu` 下自己用的配置和软件包, 方便装系统后快速折腾.

# 虚拟机下使用额外功能

比如拖拽, 共享文件, 改变窗口大小.

## Win 下 vmware

`open-vm-tools`

虚拟机下安装必不可少, 在装 `VMware Tools` 时, 官方推荐用 `open-vm-tools` , 听官方的.

```shell
apt-get install open-vm-tools
# vm-tools 安装
```

这里有点没搞懂, 我的系统是 Ubuntu 14.04LTS, 总共装了四个包才搞定, 使用 `dpkg -l | grep open-vm` 查看安装的包, 应该有多余的, 不知道哪个是多余的.

```shell
mindy@dog:~$ dpkg -l|grep open-vm
ii  open-vm-toolbox-lts-trusty  2:9.4.0-1280544-5ubuntu6.2  all Open VMware Tools for virtual machines hosted on VMware (transitional package)
ii  open-vm-tools-desktop2:9.4.0-1280544-5ubuntu6.2 amd64   Open VMware Tools for virtual machines hosted on VMware (CLI)
ii  open-vm-tools-desktop   2:9.4.0-1280544-5ubuntu6.2  amd64   Open VMware Tools for virtual machines hosted on VMware (GUI)
ii  open-vm-tools-lts-trusty    2:9.4.0-1280544-5ubuntu6.2  all Open VMware Tools for virtual machines hosted on VMware (transitional package)
```

## Mac 下 Parallels

可以自动安装

# 修改时区

运行 `sudo dpkg-reconfigure tzdata` 选择时区.

# 语言

## 终端界面改为中文提示

在 `/etc/environment` 中加入:

```conf
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh:en_US:en"
```

运行 `sudo dpkg-reconfigure locales`, 重启生效.

## 个人目录下文件夹名称改为英文

中文系统下, 终端中有中文目录会十分蛋疼.

```shell
export LANG=en_US
xdg-user-dirs-gtk-update
```

据我观察 `xdg-user-dirs-gtk-update` 命令是在 `~/.config/user-dirs.dirs` 中写入了配置.

# 搜狗输入法

以前用的 ibus 的谷歌拼音很棒, 不过貌似不怎么维护, 用搜狗!

官网下载 `.deb` 包, 安装后更改输入法 `ibus` 为 `fcitx`, 重启即可.


杂项:

- tree
- sl
- zsh
- https://github.com/robbyrussell/oh-my-zsh
- vim
- git
