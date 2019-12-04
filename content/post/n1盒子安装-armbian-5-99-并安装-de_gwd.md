---
title: 'N1盒子安装 Armbian 5.99,安装 De_gwd，旁路由。'
date: 2019-11-30T11:22:47.248Z
lastmod: 2019-11-30T11:22:47.298Z
---
## 一、De_GWD介绍

### 1.名字解析
理解了名字，你其实也就能知道这套系统是干什么的了。
    
- De: Debian系统，linux 的一个分支。     
- GW: gateway，中文网关，负责中转国内国外流量。  
- D: DNS,域名解析系统，对抗 DNS 域名污染。     

### 2.功能
- V2ray tls 1.3 ws   
- pi-hole    
- WireGuard Server   
- 内网分流
- CloudFlare DDNS 
### 3.客户端界面

![](https://img.suan.su/Screen-Shot-2019-12-03-20-40-29.31.png)
### 3.工作原理
DNS 部分是通过 v2ray tls we 的方式连接远程服务器获取DNS，所以你需要在客户端和服务端都安装De_GWD。

## 二、在境外服务器上安装De_GWD
### 1.选择 debian系统
### 2.安装
执行后，等待就行了。
```bash
apt install -y wget
bash <(wget --no-check-certificate -qO- https://raw.githubusercontent.com/jacyl4/de_GWD/master/server)
```
### 3.配置参数

## 三、在 N1上安装De_GWD 安装
### 1.在 N1上安装Debian系统
[N1参考]<https://yuerblog.cc/2019/10/23/%e6%96%90%e8%ae%afn1-%e5%ae%8c%e7%be%8e%e5%88%b7%e6%9c%baarmbian%e6%95%99%e7%a8%8b/> 
### 2.在 Debian安装 De_GWD
安装速度挺慢的，最好是挂梯子安装。
```bash
apt install -y wget
bash <(wget --no-check-certificate -qO- https://acccmi.cf/gwd/client)
```
### 3.配置参数

## 四、完成

好多信息没写，你很难成功。好像配置都是口口相传的，所以，你去群里面问吧。[![telegram](https://i.loli.net/2019/10/23/Ol9PX7io5b3hZsz.png)](https://t.me/de_GWD)

-----
[N1刷机教程参考]<https://yuerblog.cc/2019/10/23/%e6%96%90%e8%ae%afn1-%e5%ae%8c%e7%be%8e%e5%88%b7%e6%9c%baarmbian%e6%95%99%e7%a8%8b/>    
[De_WGD项目地址]<https://github.com/jacyl4/de_GWD>    
[安装De_WGD教程]<https://jacyl4.github.io/post/debian-gateway/>    
[类似项目]<https://github.com/wi1dcard/kexue-gateway>
