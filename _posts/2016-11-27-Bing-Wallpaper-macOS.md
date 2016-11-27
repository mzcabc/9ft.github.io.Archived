---
layout: post
title: 设置 Bing 主页背景为系统桌面
date: 2016-11-27 13:41:00 +0800
category: 技术
---
Windows 平台下有微软现成的软件.
macOS 下也不能放过 Bing 精选的壁纸.

无意中看到一个[`使用 Shell 实现的代码`](Mac OS X自动下载切换桌面壁纸), 但实际上不能使用.

基于上面链接的方法, 复习一下 Shell 编程和正则表达式, 自己修改的代码如下:

```Shell
#!/bin/sh
localDir="/Users/$USER/Pictures/BingWallpaper"
filenameRegex=".*"$(date "+%Y-%m-%d")".*jpg"
log=$localDir/log.log

# 判断本地是否存在今日壁纸
findResult=$(find $localDir -regex $filenameRegex)
if [ ! -n "$findResult" ]; then
    baseUrl="cn.bing.com"
    # 提取壁纸图片URL
    imgurl=$(expr "$(curl -L $baseUrl | grep hprichbg)" : '.*hprichbg\(.*\)",id.*')
    # 提取图片名称
    filename=$(expr "$imgurl" : '.*/\(.*\)')
    # 本地图片地址
    localpath="$localDir/$(date "+%Y-%m-%d")-$filename"
    # 下载图片至本地
    curl -o $localpath $baseUrl/az/hprichbg/$imgurl
    # 调用Finder应用切换桌面壁纸
    osascript -e "tell application \"Finder\" to set desktop picture to POSIX file \"$localpath\""
    echo "$(date +"%Y-%m-%d %H:%M:%S") Downloaded $filename" >> log
else
    echo "$(date +"%Y-%m-%d %H:%M:%S") Exist" >> log
    exit 0
fi
```

#### Curl

Using **curl** to transfer data.

**`-L, —location`**

> If  the server reports that the requested page has moved to a different location (indicated with a Location: header and  a  3XX  response code), this option will make curl redo the request on the new place

**`-o, --output <file>`**

> Write output to <file> instead of stdout.

#### Regex

**`expr STRING : REGEXP`**

> Anchored pattern match of regular expression *REGEXP* in *STRING*. [Source](Linux and Unix expr command)

Regex can test on: [regex101.com](Regex101)

**`\(.*\)`**

> The regular expression **'\\(.*\\)'** represents "The actual text (whatever appears in **between the parentheses**, which are escaped with backslashes) which matches the pattern **.\***, which itself represents any number of any character." Matched against the text **text**, this returns the string exactly:

Got `imgurl` = `BlackchurchRock_ZH-CN9991716795_1920x1080.jpg`

#### Crontab

因为学校的网络环境比较复杂, 通常开机后并没有网络连接, 所以没办法设置为开机启动. 现在用的办法是用 Crontab 每小时运行一次脚本, 如果检查到今天的壁纸已经存在, 则放弃.

**crontab** [**-u** **user**] **file**

**crontab** [**-u** **user**] { **-l** | **-r** | **-e** }

[Example of job definition:](What are the runtime permissions of a cron job?)

```shell
# Example of job definition:
.---------------- minute (0 - 59)
|  .------------- hour (0 - 23)
|  |  .---------- day of month (1 - 31)
|  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
|  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
|  |  |  |  |
*  *  *  *  * user-name  command to be executed
```

**Shell**

```shell
# -n 判断一个变量是否有值
if [ ! -n "$var" ]; then
  echo "$var is empty"
  exit 0
else
  echo "$var is not empty"
  exit 0
fi
```


[Mac OS X自动下载切换桌面壁纸]:http://www.cnblogs.com/feiqihang/p/5076573.html
[curl网站开发指南]:http://www.ruanyifeng.com/blog/2011/09/curl.html
[Linux and Unix expr command]:http://www.computerhope.com/unix/uexpr.htm
[Regex101]:https://regex101.com
[What are the runtime permissions of a cron job?]:http://unix.stackexchange.com/questions/81805/what-are-the-runtime-permissions-of-a-cron-job
[How to Display (List) All Jobs in Cron / Crontab]:https://www.liquidweb.com/kb/how-to-display-list-all-jobs-in-cron-crontab/
