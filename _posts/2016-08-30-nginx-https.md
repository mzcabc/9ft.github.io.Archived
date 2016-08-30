---
layout: post
title: 使用 Let’s Encrypt 为网站增加 HTTPS 访问方式
date: 2016-08-30 13:20:07 +0800
category: 技术
---
使用的证书是免费的 Let’s Encrypt, 相同免费的还有 StartSSL, 分别看了他俩的主页, 最后选择了 [Let’s Encrypt](https://letsencrypt.org/).

为了方便部署, 使用了电子前哨基金会(EFF)为 Let's Encrypt 推出的客户端 [Certbot](https://certbot.eff.org).

# 下载安装 Let’s Encrypt 和 Certbot

```shell
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
./certbot-auto
```

会自动安装 certbot 和其依赖项, 包括 letsencrypt. 最后有个配置向导, 我用这个向导没成功, 下面手动配置就好.

# 证书配置和获取

**使用到的命令**

```
usage:
  certbot [SUBCOMMAND] [options] [-d domain] [-d domain] ...

Major SUBCOMMANDS:
  certonly  仅获取证书, 不安装.

optional arguments:
  --webroot  获取的证书放到 webroot 里.
  --agree-tos  同意协议.
  --email  邮箱
  -w  网站路径
  -d  域名
```

**获取证书**

```shell
./certbot-auto certonly --webroot --agree-tos --email example@gmail.com -w /usr/share/nginx/example.com -d example.com
```

运行后生成的证书在 `/etc/letsencrypt/live/` 下.

> ⚠️ 多域名单证书没成功, 以后有空再看着文档搞搞

# 配置 Nginx

**同时使用 http:// 和 https:// 访问**

```nginx
server {
  listen 80;
  listen [::]:80;
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name example.com;
  root /usr/share/nginx/example.com;
  index index.html;

  ssl_certificate      /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key  /etc/letsencrypt/live/example.com/privkey.pem;
}
```

**http 跳转到 https**

```nginx
server {
  listen 80;
  listen [::]:80;
  server_name example.com;
  return 301 https://$server_name$request_uri;
}

server {
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name example.com;
  root /usr/share/nginx/example.com;
  index index.html;

  ssl_certificate      /etc/letsencrypt/live/example.com/fullchain.pem;
  ssl_certificate_key  /etc/letsencrypt/live/example.com/privkey.pem;
}
```

使用 `nginx -s reload` 测试并重载配置.

# 完成

观看效果 [个人博客](http://canukiss.me).

![canukiss.me via HTTPS](/assets/img/post/2016-08-30-nginx-https/canukiss.me_via_https.png "canukiss.me via HTTPS")
