---
layout: post
title: 自定义安装 Office 2016
date: 2016-12-04 14:36:00 +0800
category: 技术
---
安装 Office 2016 的时候没有了自定义安装的选项, 默认安装全家桶.

![](/assets/img/post/2016-12-04-office-install/office_before.jpg "Before")

**解决方法**

下载 [`Office 2016 Deployment Tool`](https://www.microsoft.com/en-us/download/details.aspx?id=49117)

运行后会生成 `setup.exe` 和 `configuration.xml`.

编辑 `configuration.xml`, 配置 `安装文件路径`，`版本`，`语言`，`要排除的组件`, 下面是我的配置文件.

```xml
<!-- Install Word, Excel, PowerPoint -->
<Configuration>
  <Add SourcePath="E:\" OfficeClientEdition="64">
    <Product ID="ProPlusRetail">
      <Language ID="zh-CN" />
      <ExcludeApp ID="Access" />
      <ExcludeApp ID="Groove" />
      <ExcludeApp ID="InfoPath" />
      <ExcludeApp ID="Lync" />
      <ExcludeApp ID="OneNote" />
      <ExcludeApp ID="Outlook" />
      <ExcludeApp ID="Publisher" />
      <ExcludeApp ID="SharePointDesigner" />
    </Product>
  </Add>
</Configuration>
```

管理员方式运行命令提示符, 运行 `setup.exe /configure configuration.xml`.

![](assets/img/post/2016-12-04-office-install/office_after.png "After")

搞定!
