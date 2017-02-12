---
layout: post
title: macOS 原生读写挂载 NTFS 文件系统
date: 2016-11-23 15:57:00 +0800
update_date: 2017-02-12 10:48:43 +0800
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

**此方法使用三个月后**

NTFS 分区时不时会出错, 具体情况是看不到文件, 不知道 macOS 下有什么软件可以修复 NTFS 的错误, 所以每次都要去 Win 下修复. 看来苹果自己的 NTFS 确实有问题. 忍无可忍入正 Paragon NTFS for Mac !
