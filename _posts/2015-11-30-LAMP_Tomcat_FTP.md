---
layout: post
title: Ubuntu 的 LAMP Tomcat FTP 安装和配置
category: 技术
---
搭建好多遍, 都没有解决 Tomcat 间歇性挂掉的问题.

日志里是这样的:

```shell
SEVERE: The web application [/app] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak, the JDBC Driver has been forcibly unregistered.

Dec 02, 2015 3:52:50 AM org.apache.catalina.loader.WebappClassLoader clearReferencesThreads

SEVERE: The web application [/app] appears to have started a thread named [MySQL Statement Cancellation Timer] but has failed to stop it. This is very likely to create a memory leak.
```
