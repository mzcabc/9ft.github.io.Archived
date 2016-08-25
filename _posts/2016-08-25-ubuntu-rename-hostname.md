---
layout: post
title: Ubuntu 下更改 hostname
date: 2016-08-25 15:27:49 +0800
category: 技术
---
有个同学在搞 Spark 要改主机名. 以前没关注过这个东西, 所以不知道怎么改.

今天用命令行的时候一直盯着主机名看, 感觉我也应该改一下, 尤其是连 VPS 时, 主机名难看的不行, 一般都是 localhost 或者一大串云服务商设定的一串.

这里更改 Ubuntu 14.04 为例.

输入命令 `man hostname` 里面有说

> **SET NAME**
>
> When  called  with one argument or with the --file option, the commands       set the host name  or  the  NIS/YP  domain  name.   hostname  uses  the       sethostname(2)  function,  while all of the three domainname, ypdomain‐       name and nisdomainname use setdomainname(2).  Note, that this is effec‐       tive  only  until  the  next  reboot.  **Edit /etc/hostname for permanent       change.**
>
> Note, that only the super-user can change the names.

**桌面系统更改 hostname**

Parallels 虚拟机快速安装的 Ubuntu 主机名是 ubuntu, 将 `/etc/hostname` 内容更改为 `virtual-ubuntu`, 重启更改生效.

**VPS 更改 hostname**

BandwagonHost 上面有一个 VPS 怎么改都不生效, 重启后 hostname 还会恢复成 localhost, 可能是服务提供商每次开机都会重置 /etc/hostname 文件, 那上面的办法就行不通了.

在 `/etc/rc.local` 中添加一行 `hostname myHostname` 即可.

连接 SSH 看起来更炫酷了是么.

![rename hostname](/assets/img/post/2016-08-25-ubuntu-rename-hostname/2016-08-25 15.09.31.png "rename hostname")
