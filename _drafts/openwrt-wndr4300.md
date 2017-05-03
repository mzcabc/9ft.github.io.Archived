# Netgear WNDR4300 OpenWrt

固件地址:
[https://downloads.openwrt.org/chaos_calmer/15.05.1/ar71xx/nand/](https://downloads.openwrt.org/chaos_calmer/15.05.1/ar71xx/nand/)

`openwrt-15.05.1-ar71xMx-nand-wndr4300-squashfs-sysupgrade.tar`

或

`openwrt-15.05.1-ar71xx-nand-wndr4300-ubi-factory.img`

img 格式用 tftp 刷入, tar 通过 Openwrt Web 端刷入

恢复模式

按住 reset 通电

echo -e "binary\nrexmt 1\ntimeout 60\ntrace\nput openwrt-15.05.1-ar71xx-nand-wndr4300-ubi-factory.img\n" | tftp 192.168.1.1


luci-i18n-base-zh-cn

http://openwrt-dist.sourceforge.net/


http://demon.tw/hardware/openwrt-usb-storage-samba.html
