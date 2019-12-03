---
title: 通过iptables实现国内 nat的IP端口数据包转发服务
date: 2019-12-03T05:41:53.036Z
lastmod: 2019-12-03T05:41:54.394Z
---

## 一、前言

俗话说：『一如电信深似海，从此小鸡不好买。』
家里是中国电信的网络，为了能跑满宽带（100M），不断尝试买不同的小鸡，通过不断试验，发现速度是CN2 GIA >CN2 > 163。家里是电信网络的人，选小鸡一定要有 CN2，不能什么车都上。如果你是联通网络，那么恭喜你，随便买吧，都不会太差。

这个教程是当时折腾国内中转方案的时候用到的，现在分享给你。

## 二、iptables
### 1.安装 iptables
```bash
#停止防火墙
systemctl stop firewalld.service
#关闭防火墙  
systemctl disable firewalld.service
#通过 yum 安装 iptables  
yum install iptables-services -y  
#启动 iptables
systemctl enable iptables.service  
```
### 2.开启端口转发功能

```bash
#添加net.ipv4.ip_forward = 1到cloudiplc.conf    
echo "net.ipv4.ip_forward = 1" >> /usr/lib/sysctl.d/cloudiplc.conf
#启动cloudiplc.conf
sysctl -p /usr/lib/sysctl.d/cloudiplc.conf
```
## 三、iptales 规则
请根据自己的需求选择
### 1.同端口号配置方案      
使用本地服务器的10000端口来转发目标IP为1.1.1.1的10000端口
```bash
-A PREROUTING -p tcp -m tcp --dport 10000 -j DNAT --to-destination 1.1.1.1
-A PREROUTING -p udp -m udp --dport 10000 -j DNAT --to-destination 1.1.1.1
-A POSTROUTING -d 1.1.1.1 -p tcp -m tcp --dport 10000 -j SNAT --to-source 本地服务器IP
-A POSTROUTING -d 1.1.1.1 -p udp -m udp --dport 10000 -j SNAT --to-source 本地服务器IP
```
### 2.非同端口号配置方案    
使用本地服务器的60000端口来转发目标IP为1.1.1.1的50000端口
```bash
-A PREROUTING -p tcp -m tcp --dport 60000 -j DNAT --to-destination 1.1.1.1:50000
-A PREROUTING -p udp -m udp --dport 60000 -j DNAT --to-destination 1.1.1.1:50000
-A POSTROUTING -d 1.1.1.1 -p tcp -m tcp --dport 50000 -j SNAT --to-source 本地服务器IP
-A POSTROUTING -d 1.1.1.1 -p udp -m udp --dport 50000 -j SNAT --to-source 本地服务器IP
```

### 3.多端口转发配置方案     
将本地服务器的10000~65535转发至目标IP为1.1.1.1的10000~65535端口
```bash
-A PREROUTING -p tcp -m tcp --dport 10000:65535 -j DNAT --to-destination 1.1.1.1
-A PREROUTING -p udp -m udp --dport 10000:65535 -j DNAT --to-destination 1.1.1.1
-A POSTROUTING -d 1.1.1.1 -p tcp -m tcp --dport 10000:65535 -j SNAT --to-source 本地服务器IP
-A POSTROUTING -d 1.1.1.1 -p udp -m udp --dport 10000:65535 -j SNAT --to-source 本地服务器IP
```
### 4.保存并重启

```bash
service iptables save
service iptables restart
```
## 四、完成 
​:tada:​ 祝你好运。

-----

[参考1]<https://www.cloudiplc.com/knowledgebase.php?action=displayarticle&id=9>   
[参考2]<https://moe.best/vps-domain/cloudiplc-nat.html>
