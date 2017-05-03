# Installation

ngrok 商业化了, 官网下载的最近版本不能连自己的服务端, 现在只能自行编译.

**golang环境**

```shell
apt-get install golang
```

**获取 ngrok 源码**

```shell
git clone https://github.com/inconshreveable/ngrok.git
```

**编译各平台客户端和服务端**

```shell
cd ngrok

NGROK_DOMAIN="*.example.com"

openssl genrsa -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pem
openssl genrsa -out devMice.key 2048
openssl req -new -key device.key -subj "/CN=$NGROK_DOMAIN" -out device.csr
openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days 5000

\cp rootCA.pem assets/client/tls/ngrokroot.crt -f
\cp device.crt assets/server/tls/snakeoil.crt  -f
\cp device.key assets/server/tls/snakeoil.key -f

GOOS=linux GOARCH=amd64 make release-server
GOOS=linux GOARCH=amd64 make release-client
GOOS=windows GOARCH=amd64 make release-client
GOOS=linux GOARCH=arm make release-client
GOOS=darwin GOARCH=amd64 make release-client
```

**Openwrt 的客户端**

https://github.com/dosgo/ngrok-c

编译好的 ngrok-c, 和 LuCI 界面放到 Openwrt 上:

```shell
wget https://www.shintaku.cc/files/ngrokc_ba56781152-1_ar71xx.ipk
wget https://www.shintaku.cc/files/luci-app-ngrokc_git-15.290.16504-8c2fd44-1_all.ipk
```

如果安装时遇到问题请确定路由器上是否已经安装了必要的库：

```shell
opkg install libstdcpp
opkg install libopenssl
```

# 配置

## 服务端

```shell
./ngrokd -domain="mindy.tk" -httpAddr=":8777" -httpsAddr=":8778"
```

-tlsKey=./device.key -tlsCrt=./device.crt    ????

测试后丢到开机启动中

## 客户端

**Linux 中**

测试 ngrok 是否可用

```shell
./ngrok -log=log.txt -config=./ngrok.cfg -subdomain=test 80
```

`ngrok.cfg` 中

```yml
server_addr: "server.mindy.tk:4443"
trust_host_root_certs: false

tunnels:
  ssh:
    remote_port: 22002
    proto:
      tcp: "22"
  web:
    subdomain: "yak.server"
    proto:
      http: "127.0.0.1:80"
  HomeAssistant:
    subdomain: "ha.yak.server"
    auth: "mindy:1!"
    proto:
      http: "8123"
```

启动

```shell
./ngrok -config=./ngrok.cfg start ssh web HomeAssistant
```

openwrt 客户端网页中设置即可

配置文件为

```
root@OpenWrt:~# cat /etc/config/ngrokc

config servers 'bat_server'
	option host 'mindy.tk'
	option port '4443'

config tunnel 'openwrt'
	option server 'bat_server'
	option type 'http'
	option lport '80'
	option custom_domain '0'
	option dname 'giraffe.server'
	option enabled '1'

config tunnel 'ssh'
	option enabled '1'
	option server 'bat_server'
	option type 'tcp'
	option lport '22'
	option rport '22001'
```

# 服务器端端口转发




https://xicheng412.github.io/2016/09/27/ngrok-config/
https://imququ.com/post/self-hosted-ngrokd.html
