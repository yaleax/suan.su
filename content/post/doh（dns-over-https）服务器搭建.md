---
title: DOH（DNS-over-HTTPs）服务器搭建
date: 2019-12-01T09:49:00.196Z
lastmod: 2020-02-23T03:04:10.974Z
---

## 一、Doh介绍
DOH全称为DNS-over-HTTPs，顾名思义，其主要目的是使用https协议来进行DNS请求。
正常的DNS请求过程是通过计算机上的DNS客户端程序来帮助用户发起DNS请求，而不是通过浏览器本身发送，DNS使用的协议是UDP协议，UDP协议不具备很好的安全性，这样发起的DNS请求会遭到DNS劫持，攻击者会将用户想要访问的域名解析到别的IP地址上，因此DOH为了解决这个问题而出现。
DOH是使用HTTPs协议发送dns请求，请求到达DOH服务器后，由DOH服务器解码HTTPS并发送DNS请求，DNS请求结果返回到DOH服务器上后，再由将其打包成HTTPS返回给客户端，这就保证了客户端发起的dns请求不会被攻击者拿到。


## 二、Ubuntu和 debian 系统  
### 1.更新系统
```bash
apt update
apt -y upgrade
```
## 三、Pihole
### 1.安装 pihole

```bash
curl -sSL https://install.pi-hole.net | bash
```
### 2.修改 pihole 配置文件
```bash
rm /etc/dnsmasq.d/01-pihole.conf
nano /etc/dnsmasq.d/01-pihole.conf
```
### 3.Pihole的配置文件
```bash
addn-hosts=/etc/pihole/gravity.list
addn-hosts=/etc/pihole/black.list
addn-hosts=/etc/pihole/local.list


localise-queries


no-resolv

#log-queries
log-facility=/var/log/pihole.log

local-ttl=2

log-async
domain-needed
bogus-priv
server=1.1.1.1
server=8.8.8.8
server=1.0.0.1
server=8.8.4.4
interface=ens3

```
### 4.重新启动 pihole 并查看状态
```bash
pihole restartdns
pihole status
```
## 四、Lighttpd
### 1.修改lighttpd配置文件

```bash
nano /etc/lighttpd/lighttpd.conf
```
### 2.修改server.port值

```bash
    vim /etc/lighttpd/lighttpd.conf
    #找到以下字段，修改server.port值
    ...
    server.groupname = "www-data"
    server.port = 9090
    accesslog.filename = "/var/log/lighttpd/access.log"
    ...
```
### 3.重启 lighttpd
```bash
systemctl restart lighttpd
```
### 4.更改管理员密码

```bash
pihole -a -p
```
访问 http://youip:9090/admin

## 五、DNS-over-HTTPs
### 1.安装DNS-over-HTTPs服务器

```bash
wget https://ning.su/admin/api/public/dl/QueBWSnt/doh-server_2.0.1_amd64.deb
sudo dpkg -i doh-server_2.0.1_amd64.deb
```

### 2.修改DNS-over-HTTPs配置
```bash
rm nano /etc/dns-over-https/doh-server.conf
nano /etc/dns-over-https/doh-server.conf
```
### 3.DNS-over-HTTPs的配置文件
```conf
listen = [ "127.0.0.1:8053", ]

path = "/"

upstream = [ "udp:127.0.0.1:53", ]

timeout = 10
tries = 3
verbose = false
log_guessed_client_ip = false
```

### 4.重新启动DNS-over-HTTPs并重看状态

```bash
systemctl restart doh-server
systemctl status doh-server
```
## 六、Caddy  
默认你已经安装了 Caddy,如果没有安装，可以参考本博客关于 Caddy 的文章。
### 1.修改 Caddy 的Caddyfile 文件
```bash
nano /usr/local/caddy/Caddyfile
```
### 2.Caddyfile配置文件
```bash
proxy   /      http://127.0.0.1:8053 {
   header_upstream Host {host}
   header_upstream X-Real-IP {remote}
   header_upstream X-Forwarded-For {>X-Forwarded-For},{remote}
   header_upstream X-Forwarded-Proto {scheme}
}
```
### 3.重新启动 Caddy并查看状态

```bash
/etc/init.d/caddy restart
/etc/init.d/caddy status
```
------
## 七、参考文章
[Pihole]<https://ywnz.com/linuxyffq/4332.html>
[lighttpd]<https://azhuge233.com/%E6%9B%B4%E6%94%B9-pi-hole-%E7%BD%91%E9%A1%B5%E7%9B%91%E5%90%AC%E7%AB%AF%E5%8F%A3%E5%92%8C%E7%AE%A1%E7%90%86%E5%91%98%E5%AF%86%E7%A0%81/#geng_gai_jian_ting_duan_kou>
[Doh]<https://blog.csdn.net/qq_39378221/article/details/103245108>
[Caddy]<https://ning.su/posts/hugo-caddy-v2ray-on-ubuntu/#caddy>
