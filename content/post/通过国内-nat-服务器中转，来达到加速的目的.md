---
title: 通过iptables实现国内 nat的IP端口数据包转发服务
date: 2019-12-03T05:41:53.036Z
lastmod: 2019-12-03T05:41:54.394Z
---

## 一、前言

俗话说：『一如电信深似海，从此小鸡不好买。』
家里是中国电信的网络，为了能跑满宽带（100M），不断尝试买不同的小鸡，通过试验，发现速度是CN2 GIA >CN2 > 163。家里是电信网络的人，选小鸡一定要上CN2，不要心存幻想。如果你是联通网络，那么恭喜你，随便买吧，都不会太差。

或者你不认命，用国内联通 nat 小鸡中转一下？这个教程是当时折腾国内中转方案的时候用到的转发方法，现在分享给你。

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
#添加net.ipv4.ip_forward = 1到sysctl.conf    
echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
#查看
sudo sysctl -p

```

### 3.查看本级ip地址

```bash
ip a
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

### 4.删除端口

```bash
#删除
sudo iptables -t nat -D PREROUTING -p tcp --dport 21998 -j DNAT --to-destination 1.1.1.1:443
sudo iptables -t nat -D PREROUTING -p udp --dport 21998 -j DNAT --to-destination 1.1.1.1:443
sudo iptables -t nat -D POSTROUTING -p tcp -d 1.1.1.1 --dport 443 -j SNAT --to-source 本地服务器IP
sudo iptables -t nat -D POSTROUTING -p udp -d 1.1.1.1 --dport 443 -j SNAT --to-source 本地服务器IP
```

## 四、保存并重启

```bash
sudo apt install iptables-persistent -y
sudo netfilter-persistent save
sudo netfilter-persistent start
```
## 五、查看当前运行端口
```bash
​iptables -t nat -nL
```



-----

[参考1]<https://www.cloudiplc.com/knowledgebase.php?action=displayarticle&id=9>   
[参考2]<https://moe.best/vps-domain/cloudiplc-nat.html>
