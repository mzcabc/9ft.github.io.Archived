我的智能家居

有树莓派，有 iOS，不折腾 HomeKit 浪费了。

树莓派我装的系统是 Ubuntu Mate，方便折腾。

# 1. Home Assistant

Home Assistant 有两种安装方式，安装操作系统 Hassbian 会自带 Home Assistant 或者直接安装 Home Assistant. 网上 99% 教程都是安装系统，也是醉了。

[Installation on Your Computer - Home Assistant](https://home-assistant.io/docs/installation/python/)
```shell
pip3 install homeassistant
hass --open-ui # 或 python3 -m homeassistant --open-ui
```

通过 http://localhost:8123 访问界面。

`~/.homeassistant/configuration.yaml` 是其配置文件。改后重启 homeassistant 生效。

现在可以试着添加一个 Automation entry [玩一下 ifttt](https://home-assistant.io/components/ifttt/)

接着可以玩一下 [RESTful API](https://home-assistant.io/developers/rest_api/)，畅想一下未来。

# 2. Homebridge

使用 Homebridge 就可以使用 HomeKit 了。

https://github.com/nfarina/homebridge#installation

```shell
sudo npm install -g --unsafe-perm homebridge
homebridge
```

安装一个 plugin [homebridge-weather](https://www.npmjs.com/package/homebridge-weather) 作为第一个传感器。

https://github.com/nfarina/homebridge/issues/163#issuecomment-140776486

# 3. Home Assistant for Homebridge

使 Home Assistant 可以通过 Homebridge。

https://www.npmjs.com/package/homebridge-homeassistant

# 4. Home Assistant 中添加小米网关

[HomeAssistant Xiaomi Hub Component by Rave (Lazcad)](https://github.com/lazcad/homeassistant)

https://github.com/fritz-smh/yi-hack/issues/118#issuecomment-272024337
