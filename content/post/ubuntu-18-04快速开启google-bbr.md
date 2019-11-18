---
title: Ubuntu 18.04快速开启Google BBR
date: 2019-11-18T00:32:32.141Z
lastmod: 2019-11-18T00:32:32.256Z
tags:
  - Linux
---
## 前言

Ubuntu 18.04前几天发布了，改变挺大的，内核也直接升到了正式版4.15，而BBR内核要求为4.9，也就是说满足了，所以我们不需要换内核就可以很快的开启BBR，这里简单说下方法。

## 命令

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

:tada:完成！
