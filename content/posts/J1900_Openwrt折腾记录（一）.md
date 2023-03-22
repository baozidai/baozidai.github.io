+++ 
draft = false
date = 2023-03-08T13:48:00+08:00
title = "J1900|OpenWrt安装配置笔记"
description = "记录折腾J1900主板的事"
slug = ""
authors = []
tags = ["技术笔记"]
categories = []
externalLink = ""
series = []
+++

# J1900|OpenWrt安装配置笔记

这份笔记记录的信息可能不完全

--------------

我使用的固件是openwrt-22.03.3-x86-generic-generic-ext4-combined-efi

## 如何启动

1) 需要把主板的uefi引导关闭，保证能够从U盘或者其他外部介质启动。

2) 使用了`balenaEtcher`来将openwrt的固件烧录到U盘中，`Roadkil's Disk Image`似乎也可以，但是效果不如BlaenaEtcher。当然，BlaenaEtcher不会使用全部的U盘空间。这篇文章可以实现扩容[OpenWrt 存储空间扩容的两种方案](https://www.openwrt.pro/post-594.html) 这个内容可以让你把根目录`/`挂载在U盘的其他地方。

## 关于如何通过网线调试并且上网

用一根网线连接

我的笔记本<--------->1900主板

然后在Windows控制面板——适配器——选择可以联网的适配器——右键——属性——共享——对“以太网”共享此连接。在openwrt里面把lan的网关设置为电脑有线适配器的ip地址，然后1900就可以通过电脑的网络上网了。这适用于J1900没有多余网口时候的调试。

## 中文化和其他驱动

安装luci-i18n-base-zh-cn ~~luci-i18n-firewall-zh-cn~~（安装了之后看不出哪里汉化了）。我的无线USB网卡是W311U+，内置的是RT2870STA的驱动，在opkg里面搜到了这个驱动 kmod-rt2800-usb。安装驱动之后，如果有必要，使用

```bash
ifconfig wlan<x> up
```

把无线网卡启动起来。`wifi config`命令可以自动检测无线设备（前提是安装了恰当的驱动），并在/etc/config/wireless生成配置文件，需要在这个配置文件中将无线打开（默认是关闭的）

## 关于DMZ主机

在防火墙下面添加如下规则`cat /etc/config/firewall`

```bash
config redirect 'dmz'
        option name 'dmz'
        option src 'wan'
        option proto 'tcp'
        option target 'DNAT'
        option dest_ip '192.168.18.235'
        option enabled '1'

config redirect 'dmzudp'
        option name 'dmzudp'
        option src_port '!67'
        option src 'wan'
        option proto 'udp'
        option target 'DNAT'
        option dest_ip '192.168.18.235'
        option enabled '1'
```

## 登陆校园网

安装了`wget-ssl`,之后如下命令可以登陆校园网

```shell
wget --no-check-certificate --quiet \
  --method POST \
  --timeout=0 \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --body-data 'DDDDD=<学号>&upass=<密码hash:通过抓包可以取得>&R1=0&R2=1&para=00&0MKKey=123456' \
   '10.254.7.4'
```

shell 延迟命令

```shell
sleep <seconds> && <some thing>
```

## 其他提示

如果没有`挂载点`选项

```shell
opkg install block-mount
```

修改挂载点之后

```shell
mkdir -p /tmp/introot
mkdir -p /tmp/extroot
mount --bind / /tmp/introot
mount /dev/sda<分区好> /tmp/extroot # 一定主要这里的分区号，要填写正确，默认给的始终是sda1
tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
umount /tmp/introot
umount /tmp/extroot
```

安装hostapd，无线可以设置密码

IPv6中继要在WAN打开dhcpv6但是关闭dhcpv4，全选中继然后设置为主设备
