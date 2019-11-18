---
title: 利用Dnsmasq SNIproxy解锁 Netflix在 Ubuntu18.04 服务器上
date: 2019-11-17T08:41:44.682Z
lastmod: 2019-11-17T08:41:45.953Z
tags:
  - Netflix
  - Linux
---

## 一、原理
Dnsmasq SNIproxy一键脚本，是利用Dnsmasq的DNS将网站解析劫持到SNI proxy反向代理的页面上。
## 二、必要条件
### 1.可以看 Netlify的服务器
### 2.想要解锁的服务器
## 三、教程第1部分
在可以看 Netlify的服务器上输入下面的命令
### 1.关闭dnsmasq[^footnote]
[^footnote]:[关闭Dnsmasq来源](https://superuser.com/questions/1318220/ubuntu-18-04-disable-dnsmasq-base-and-enable-full-dnsmasq)
```bash
nano /etc/systemd/resolved.conf
```
### 2.修改文件
```config
DNSStubListener=no
```
### 3.重启服务器
```bash
reboot
```
### 4.安装解锁工具[^footnote2]
[^footnote2]:[解锁 Netflix来源](https://www.mebi.me/1035)
```bash
wget --no-check-certificate -O dnsmasq_sniproxy.sh https://raw.githubusercontent.com/myxuchangbin/dnsmasq_sniproxy_install/master/dnsmasq_sniproxy.sh && bash dnsmasq_sniproxy.sh -i
```
{{% notice info 卸载命令 %}}
wget --no-check-certificate -O dnsmasq_sniproxy.sh https://raw.githubusercontent.com/myxuchangbin/dnsmasq_sniproxy_install/master/dnsmasq_sniproxy.sh && bash dnsmasq_sniproxy.sh -u
{{% /notice %}}
### 5.重启服务器
```bash
reboot
```
## 四、教程第2部分
想要解锁的服务器上输入下面的命令
### 1. 设置 ip
把下面代码里面的 ip 更改成，可以看 netflix 的服务器 ip。
```bash
echo nameserver IP > /etc/resolv.conf
```
### 2. 查看是否成功
如果返回的 ip 是可以看 netflix 的服务器 ip，那么就说明成功了。
```bash
ping -c4 netflix.com
```
:tada:完成\
