---
layout: post
title: 为路由器安装 LEDE
date: 2017-10-22 19:22 +0800
category: 技术
tags:
keywords:
description:
---

LEDE 可以看作是 OpenWrt 的新版本. 我的路由器是 Netgear WNDR4300, 其他路由使用 LEDE 也是一个套路.

> [Techdata: Netgear WNDR4300 v1](https://lede-project.org/toh/hwdata/netgear/netgear_wndr4300_v1)

# 安装

找到固件下载链接

如 `lede-17.01.4-ar71xx-nand-wndr4300-ubi-factory.img` 或 `lede-17.01.4-ar71xx-nand-wndr4300-squashfs-sysupgrade.tar`

img 格式用 tftp 刷入, tar 通过 LEDE Web 端刷入

## 进入恢复模式

按住路由的 reset 通电

```shell
echo -e "binary\nrexmt 1\ntimeout 60\ntrace\nput lede-17.01.4-ar71xx-nand-wndr4300-ubi-factory.img\n" | tftp 192.168.1.1
```

浏览器访问 http://192.168.1.1/ 应该能跳转到 luci 界面.

## 中文界面

```shell
opkg list | grep base-zh-cn
opkg install luci-i18n-base-zh-cn
```

# 使用

## 安装必要的软件包

(你懂的...可能是安装 LEDE 最实用的功能)

已经有人将软件包整合好了 [`OpenWrt-dist`](http://openwrt-dist.sourceforge.net).

如果可以直连互联网

`/etc/opkg.conf` 中, 添加源:
```
src/gz openwrt_dist http://openwrt-dist.sourceforge.net/packages/LEDE/base/ar71xx
src/gz openwrt_dist_luci http://openwrt-dist.sourceforge.net/packages/LEDE/luci
```

并安装

```
opkg update
opkg install *
```

如果不能直连互联网, 在 `http://openwrt-dist.sourceforge.net/packages/LEDE/` 手动下载安装.

或者参照 [https://cokebar.info/archives/664](https://cokebar.info/archives/664) 设置.
