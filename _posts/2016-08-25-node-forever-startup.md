---
layout: post
title: 利用 forever 设置 Node.js 项目自启动
date: 2016-08-25 14:03:47 +0800
category: 技术
tags:
keywords:
description:
---
最近在 VPS 上跑一个 Node.js App, 需要后台运行, 开机自启动.

**后台运行**

想要程序在关闭终端后一直运行运行 `nohup node index.js` 即可.

或者安装一个专门管理 Node.js 运行的软件, 我这里安装了 [`forever`](https://github.com/foreverjs/forever) (同类型软件还有 [`pm2`](https://github.com/Unitech/pm2)).

**forever 开机自启**

随便网上查一下, 得到的资料都是写一个 service 或者 crontab 定时任务, 这些资料太老旧, 以后维护或者更换主机重新配置都是麻烦. 其实大可以不必. 直接将 forever 命令放到启动项里就好.

`/etc/rc.local` 中按自己实际路径添加即可.

```conf
/usr/bin/forever start -a -l /root/App/log.log -o /root/App/out.log -e /root/App/err.log /root/App/index.js
```

- forever 目录: 可以用 `whereis forever` 查询
- -a 参数: 表示日志可以追加, 如果已有同名日志文件启动时会报错.
- -l -o -e 日志目录.

重启后使用 `forever list` 命令验证是否自启成功.
