---
layout: post
title: macOS 原生读写挂载 NTFS 文件系统
date: 2016-11-23 15:57:00 +0800
category: 技术
---
Paragon NTFS for Mac® 虽然很好用, 最近重装系统尝试了一下[知乎答案](https://www.zhihu.com/question/19571334/answer/89658747)的方法, 目前已经稳定使用一个月, 此文记录一下挂载过程.

```shell
$ sudo -s	# 切换 root 用户
$ cd /sbin	# 进入 /sbin 文件夹
$ mv mount_ntfs mount_ntfs_orig	# 重命名
$ vi mount_ntfs	# 新建 mount_ntfs 文件
```

在 `mount_ntfs` 中写入:

```shell
#!/bin/sh
/sbin/mount_ntfs_orig -o rw,nobrowse "$@"
```

```shell
$ chmod a+x mount_ntfs	# 增加可执行权限
$ exit	# 退出 root 身份
```

重启验证

⚠️ (未验证)如果遇到问题: `mv: rename mount_ntfs to mount_ntfs_orig: Operation not permitted.`

操作如下:

1) 重启, `cmd+R` 进入恢复 (recovery) 模式
2) terminal 下输入: `$ csrutil disable`
4) `$ reboot` 重启
