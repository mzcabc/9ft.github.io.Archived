---
layout: post
title: Ubuntu 下安装 LNMP
category: 技术
---

# 安装 `LAMP`

咦？我们要安装 `LNMP`, 为什么要安装 `LAMP` 呢, 因为 `Ubuntu` 安装 `LAMP` 超级方便. 要其中的 `MP (MySQL, PHP)`.

```shell
sudo apt-get install lamp-server^
```

`php5-fpm` 和 `php5-mcrypt` 必须安装. 没安装上的话要手动安装. 其中:

- `php5-fpm` 是 `FastCGI Process Manager`, 是 `Nginx` 配合 `PHP` 所必须使用的.
- `php5-mcrypt` 是 `phpMyAdmin` 需要使用到的.

# 安装配置 `Nginx`

## 安装

```shell
sudo apt-get install nginx
```

## 配置

### 部署 `HTML`

```conf
server {
  listen 80;
  listen [::]:80;
  root /usr/share/nginx/example.com;
  index index.html index.htm;
  server_name example.com;

  location / {
    try_files $uri $uri/ =404;
  }
}
```

### 部署 `PHP`

```conf
server {
  listen 80;
  listen [::]:80;
  server_name example.com;
  root /usr/share/nginx/example.com;
  index index.html index.htm index.php;

  location / {
    try_files $uri $uri/ =404;
  }

  location ~ \.php$ {
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_index index.php;
    include fastcgi_params;
  }

  location ~ /\.ht {
    deny all;
  }
}
```

`PHP` 可以通过写个探针测试配置是否正确

```php
<?php phpinfo(); ?>
```

# 安装配置 `phpMyAdmin`

## 安装

```shell
sudo apt-get install phpmyadmin
```

## 配置

仿照上一节 `部署 PHP` 中为 `phpmyadmin` 设置访问入口.

```conf
server {
  listen 80;
  listen [::]:80;
  server_name phpmyadmin.example.com;
  root /usr/share/phpmyadmin;
  index index.php;

  location / {
    try_files $uri $uri/ =404;
  }

  location ~ \.php$ {
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_index index.php;
    include fastcgi_params;
  }
  location ~ /\.ht {
    deny all;
  }
}
```

## 问题: 缺少 `mcrypt` 扩展, 请检查 `PHP` 配置

如果出现 `缺少 mcrypt 扩展. 请检查 PHP 配置.` 的提示.

执行

```shell
sudo apt-get install php5-mcrypt
```  

配置 `php.ini`, 加上 `extension=php_mcrypt.so`, 修改结果:

```conf
;;;;;;;;;;;;;;;;;;;;;;
; Dynamic Extensions ;
;;;;;;;;;;;;;;;;;;;;;;
;
; If you wish to have an extension loaded automatically, use the following

extension=php_mcrypt.so
```

# Enjoy!
