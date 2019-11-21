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
{{% notice warning 重要提示%}}
在可以看Netlify的服务器上输入下面的命令
{{% /notice %}}
### 1.关闭dnsmasq[^footnote1]
[^footnote1]:[关闭Dnsmasq](https://superuser.com/questions/1318220/ubuntu-18-04-disable-dnsmasq-base-and-enable-full-dnsmasq)
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
[^footnote2]:[解锁 Netflix](https://www.mebi.me/1035)
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
{{% notice warning 重要提示 %}}
在想要解锁的服务器上输入下面的命令
{{% /notice %}}
### 1.设置 ip
把下面代码里面的 ip 更改成，可以看 netflix 的服务器 ip。
```bash
echo nameserver IP > /etc/resolv.conf
```
### 2.查看是否成功
如果返回的 ip 是可以看 netflix 的服务器 ip，那么就说明成功了。
```bash
ping -c4 netflix.com
```
:tada: 完成\
## 五、后话
后来我发现另外一种解锁思路，现在分享在这里[V2ray路由解锁Netflix](https://blog.mojxtang.com/784/)。
### 1.原理  
在能看Netflix 的机器上安装 ss。在不能看 Netlify 的机器上使用 V2ray 连接能看Netflix 的机器上安装 ss。
### 2.优点
不占用53端口，所以 nat 机器也可以做为解锁机器了。

### 3.V2ray配置
```json
{
  "inbound": {
    "port": 10000,
    "listen": "127.0.0.1",
    "protocol": "vmess",
    "settings": {
      "clients": [{
        "id": "b50a11fc-xxxx-xxxx-xxxx-c9cb6eacbc55",
        "alterId": 64
      }]
    },
    "streamSettings": {
      "network": "ws",
      "wsSettings": {
        "path": "/v"
      }
    }
  },
  "outbound": {
    "protocol": "freedom",
    "settings": {}
  },
  "outboundDetour": [{
    "protocol": "shadowsocks",
    "settings": {
      "servers": [{
        "address": "【这里输入ss服务器的IP地址】",
        "method": "【这里输入ss服务器的加密】",
        "password": "【这里输入ss服务器的密码】",
        "port": 【这里输入ss服务器的端口】,
        "ota": false
      }]
    },
    "tag": "ban"
  }],
  "routing": {
    "strategy": "rules",
    "settings": {
      "domainStrategy": "AsIs",
      "rules": [{
        "type": "field",
        "domain": [
          "domain:fast.com",
          "domain:netflix.com",
          "domain:netflix.net",
          "domain:nflxext.com",
          "domain:nflxso.net",
          "domain:nflxvideo.net",
          "domain:nflximg.net"
        ],
        "outboundTag": "ban"
      }]
    }
  }
}
```
