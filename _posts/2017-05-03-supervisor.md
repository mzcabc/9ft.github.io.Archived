---
layout: post
title: Supervisor 基本使用
date: 2017-05-03 22:00:00 +0800
category: 技术
---
# Installation

```shell
pip install supervisor
apt-get install supervisor
```

# Configuration


- `echo_supervisord_conf` 显示默认配置.
- `echo_supervisord_conf > /etc/supervisor/supervisord.conf` 默认配置重定向到文件.
- 建立要守护程序的配置文件 `supervisor.conf`:

```
[program:t2w]
directory = /root/repos/t2w ; 程序的启动目录
command = node index.js     ; 启动命令，可以看出与手动在命令行启动的命令是一样的
autostart = true     ; 在 supervisord 启动的时候也自动启动
startsecs = 5        ; 启动 5 秒后没有异常退出，就当作已经正常启动了
autorestart = true   ; 程序异常退出后自动重启
startretries = 3     ; 启动失败自动重试次数，默认是 3
;user = root          ; 用哪个用户启动
redirect_stderr = true  ; 把 stderr 重定向到 stdout，默认 false
stdout_logfile_maxbytes = 20MB  ; stdout 日志文件大小，默认 50MB
stdout_logfile_backups = 20     ; stdout 日志文件备份数
; stdout 日志文件，需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录（supervisord 会自动创建日志文件）
stdout_logfile = /root/repos/t2w/log/log.log
stderr_logfile = /root/repos/t2w/log/err.log

; 可以通过 environment 来添加需要的环境变量，一种常见的用法是修改 PYTHONPATH
; environment=PYTHONPATH=$PYTHONPATH:/path/to/somewhere
```

- 将此配置文件 include 到 Supervisor 配置 `/etc/supervisor/supervisord.conf` 中:

```
[include]
files = /root/repos/t2w/supervisor.conf
```

## WebUI

Supervisor 配置 `/etc/supervisor/supervisord.conf` 中:

```
[inet_http_server]
port = *:9001
username = user
password = password
```

# 使用

- `supervisorctl reread` 重新读取配置文件
- `supervisorctl start t2w` 启动 t2w
- `supervisorctl status` 查看状态

尝试一下杀掉进程后还会起来一个新的.

# 遇到的问题

```
# supervisorctl start myapp
myapp: ERROR (no such process)
```

## 解决

[Supervisor not loading new configuration files](https://serverfault.com/questions/211525/supervisor-not-loading-new-configuration-files)

`sudo service supervisord reload`
