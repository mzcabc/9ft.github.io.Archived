---
layout: post
title: VPS上搭建 jekyll + nginx 博客
category: 技术
tags:
keywords:
description:
---

买个版瓦工VPS，想着把个人博客迁移过去，发现安装mysql时出错，简单的查一下，好像是因为内存不足。好吧，那就不折腾LEMP了，干脆装一个jekyll好啦。

# 安装 Ruby
这里使用`RVM`安装，参照[Installing RVM][1]

> Before any other step install mpapis public key (might need gpg2) (see security)  
`gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3`

> Install RVM (development version):  
`\curl -sSL https://get.rvm.io | bash`

> Display a list of all known rubies.  
`rvm list known`

> Install a version of Ruby (eg 2.2)  
`rvm install 2.2`

> Check this worked correctly
`ruby -v`

# 安装 Jekyll
参照 [Jekyll Quick-start Instructions][2].
```bash
gem install jekyll
jekyll new my-awesome-site
cd my-awesome-site
```

# 部署 Jekyll 生成的网站
```
jekyll build --source ~/my-awesome-site --destination /var/www/awesomeblog
```

# 配置 Nginx
复制默认配置文件  
`sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/example.com`

参照 [How To Get Started with Jekyll on an Ubuntu VPS][3] 在 `example.com` 中写一个最简单的配置文件:
```
server {
  listen 80;
  # the domain of your VPS
  server_name example.com;
  root /home/deployer/blog/current;
  index index.html
  expires 1d;
}
```

Using symlinks means it's easy to take a site offline by removing the symlink without touching the original configuration file.  
`sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/example.com`

Tests the nginx configuration.  
`sudo nginx -t`

Restart nginx.  
`sudo service nginx restart`

# 架设 `HTTP` 协议的 `Git` 服务器
安装 `sudo apt-get install git gitweb fcgiwrap`.

参照 [在 Ubuntu 系统上配置 Nginx Git 服务器][4] 和 [NGINX Configuration for Gitweb and git-http-backend][5] 中的配置.

```
# HTTP server
server {
  listen 80;
  listen [::]:80;
  server_name git.mindy.tk;
  root /usr/share/gitweb;
  access_log /var/log/nginx/git.mindy.tk.access.log;
  error_log /var/log/nginx/git.mindy.tk.error.log;

  #ssl                  on;
  #ssl_certificate      /etc/ssl/certs/certforyoursite.crt;
  #ssl_certificate_key  /etc/ssl/private/sitekey.pem;
  #ssl_session_timeout 5m;
  #ssl_protocols        TLSv1 TLSv1.1 TLSv1.2;
  #ssl_ciphers          HIGH:!ADH:!MD5;
  #ssl_prefer_server_ciphers on;

  # static repo files for cloning over https
  location ~ ^.*\.git/objects/([0-9a-f]+/[0-9a-f]+|pack/pack-[0-9a-f]+.(pack|idx))$ {
    auth_basic "Restricted";
    auth_basic_user_file /etc/nginx/passwd;

    root /var/www/git.mindy.tk;
  }

  # requests that need to go to git-http-backend
  location ~ ^.*\.git/(HEAD|info/refs|objects/info/.*|git-(upload|receive)-pack)$ {
    auth_basic "Restricted";
    auth_basic_user_file /etc/nginx/passwd;

    root /var/www/git.mindy.tk;

    fastcgi_pass unix:/var/run/fcgiwrap.socket;
    fastcgi_param SCRIPT_FILENAME   /usr/lib/git-core/git-http-backend;
    fastcgi_param PATH_INFO         $uri;
    fastcgi_param GIT_PROJECT_ROOT  /var/www/git.mindy.tk;
    fastcgi_param GIT_HTTP_EXPORT_ALL "";
    fastcgi_param REMOTE_USER $remote_user;
    include fastcgi_params;
  }

  # send anything else to gitweb if it's not a real file
  try_files $uri @gitweb;
  location @gitweb {
    root /usr/share/gitweb;
    auth_basic "Restricted";
    auth_basic_user_file /etc/nginx/passwd;

    fastcgi_pass unix:/var/run/fcgiwrap.socket;
    fastcgi_param SCRIPT_FILENAME   /usr/share/gitweb/gitweb.cgi;
    fastcgi_param PATH_INFO         $uri;
    fastcgi_param GITWEB_CONFIG     /etc/gitweb.conf;
    include fastcgi_params;
 }
}
```

使用`htpasswd`创建用户名及其密码.
```
htpasswd /etc/nginx/passwd user1
```

# 建立与本地仓库的连接
服务器上建立裸仓库
`git init --bare test.git`

注意检查一下 test.git 的权限， 如果权限不足的话， 使用这个命令设置一下权限:  
`chmod a+rw -R test.git`

设置`Hook`
```bash
cd hooks
vi post-receive
```
加入
```bash
#!/bin/bash -l
GIT_REPO=$HOME/repos/awesomeblog.git
TMP_GIT_CLONE=$HOME/tmp/git/awesomeblog
PUBLIC_WWW=/var/www/awesomeblog

git clone $GIT_REPO $TMP_GIT_CLONE
jekyll build --source $TMP_GIT_CLONE --destination $PUBLIC_WWW
rm -Rf $TMP_GIT_CLONE
exit
```

# Enjoy

[1]:https://rvm.io/rvm/install
[2]:https://jekyllrb.com/
[3]:https://www.digitalocean.com/community/tutorials/how-to-get-started-with-jekyll-on-an-ubuntu-vps
[4]:http://beginor.github.io/2016/03/12/http-git-server-on-nginx.html
[5]:http://weininger.net/configuration-of-nginx-for-gitweb-and-git-http-backend.html
