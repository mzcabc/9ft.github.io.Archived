
---
layout: post
title: 架设 HTTP 协议的 Git 服务器
date: 2016-08-01 14:25:24 +0800
category: 技术
---
搭建 Git 服务器, 网上随便一搜, 教程都是 SSH 协议上的. **本文是搭建基于 `HTTP` 协议的 `Git` 服务器.**

`HTTP` 协议 `Git` 服务器的优点:
- 省去证书的操作, 只要记用户名密码.
- 搭建时不需要创建 `git` 用户. 对我这小白来说, 多一个账户, 涉及权限的问题的时候一概不懂(现在正在系统学习 Linux).

# 安装
```shell
sudo apt-get install git gitweb nginx fcgiwrap
```
- git:
- gitweb: Git 自带 GitWeb 的 CGI 脚本
- fcgiwrap: 为 Nginx 提供 cgi 支持

# 配置

## 配置 gitweb

根据需要配置 `/etc/gitweb.conf`:

```shell
# path to git projects (<project>.git)
$projectroot = "/var/lib/git";

# directory to use for temp files
$git_temp = "/tmp";

# target of the home link on top of all pages
#$home_link = $my_uri || "/";

# html text to include at home page
#$home_text = "indextext.html";

# file with project list; by default, simply scan the projectroot dir.
#$projects_list = $projectroot;

# stylesheet to use
#@stylesheets = ("static/gitweb.css");

# javascript code for gitweb
#$javascript = "static/gitweb.js";

# logo to use
#$logo = "static/git-logo.png";

# the 'favicon'
#$favicon = "static/git-favicon.png";

# git-diff-tree(1) options to use for generated patches
#@diff_opts = ("-M");
@diff_opts = ();
```

## 配置 Nginx

`/etc/nginx/sites-available/default`:

```nginx
server {
  listen 80;
  listen [::]:80;
  server_name git.example.com;
  root /usr/share/gitweb/;

  auth_basic "Restricted";
  auth_basic_user_file /etc/nginx/passwd;

  location /index.cgi {
    include fastcgi_params;
    fastcgi_param SCRIPT_NAME $uri;
    fastcgi_param GITWEB_CONFIG /etc/gitweb.conf;
    fastcgi_pass  unix:/var/run/fcgiwrap.socket;
  }

  location / {
    index index.cgi;
  }
}
```

### 使用 `htpasswd` 创建用户

使用 `htpasswd` 创建用户名及其密码.

```shell
htpasswd /etc/nginx/passwd user1
```

如果没有 `htpasswd` 命令, 还有[替代方法](https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-nginx-on-ubuntu-14-04):

```shell
sudo sh -c "echo -n 'user:' >> /etc/nginx/passwd"
sudo sh -c "openssl passwd -apr1 >> /etc/nginx/passwd"
```

此时打开 `git.example.com` 应该会有 `gitweb` 的界面.
