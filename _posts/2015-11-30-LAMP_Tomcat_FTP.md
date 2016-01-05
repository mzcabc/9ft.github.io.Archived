---
layout: post
title: Ubuntu的LAMP+Tomcat+FTP安装
category: 技术
tags:
keywords:
description:
---

~~连续折腾了三天LAMP+TOMCAT，主要问题是TOMECAT间歇性挂掉。不知道问题出在哪。~~
日志里是这样的：
```
SEVERE: The web application [/app] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak, the JDBC Driver has been forcibly unregistered.

Dec 02, 2015 3:52:50 AM org.apache.catalina.loader.WebappClassLoader clearReferencesThreads

SEVERE: The web application [/app] appears to have started a thread named [MySQL Statement Cancellation Timer] but has failed to stop it. This is very likely to create a memory leak.
```
