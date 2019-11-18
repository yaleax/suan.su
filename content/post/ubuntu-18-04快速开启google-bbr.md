---
title: Ubuntu 18.04快速开启Google BBR
date: 2019-11-18T00:32:32.141Z
lastmod: 2019-11-18T00:32:32.256Z
tags:
  - Linux
---
## 前言
Google BBR 是一款免费开源的TCP拥塞控制传输控制协议, 可以使 Linux 服务器显著提高吞吐量和减少 TCP 连接的延迟。 [项目地址]<https://github.com/google/bbr>

## 教程

### 1.修改系统变量

```bash
sudo -i
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
```

### 2.保存生效

```bash
sysctl -p
```

### 3.查看内核是否已开启BBR

```bash
sysctl net.ipv4.tcp_available_congestion_control
```

显示以下即已开启：

```bash
net.ipv4.tcp_available_congestion_control = reno cubic bbr
```

### 4.查看BBR是否启动

```bash
lsmod | grep bbr
```

显示以下即启动成功：

```bash
tcp_bbr                20480  1
```

:tada: 完成！
