---
title: 谷歌云或 ubuntu系统打开 root 用户密码登录服务器
author: Suan
date: 2019-11-17T10:28:30.492Z
lastmod: 2019-11-17T10:39:34.478Z
tags:
  - Linux
---
## 前言

开通谷歌云免费试用后，发现只能用网页登录服务器，太不方便，所以记录一下解决办法。

## 教程 

### 1.切换到 root

```bash
#切换到 root 用户
sudo -i
```
### 2.编辑ssh配置文件

```bash
#使用nano编辑 sshd_config 文件
nano /etc/ssh/sshd_config
```
### 3.修改以下内容即可

```bash
#打开 root 通过密码用户登录
PermitRootLogin yes
PasswordAuthentication yes
```
{{% notice tip 提示 %}}  
修改完成后，你需要同时按 <kbd>ctrl</kbd> + <kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按<kbd>enter</kbd>保存。  
{{% /notice %}}

### 4.给root用户密码
输入2次密码
```bash
passwd root
```

### 5.重启ssh

```bash
#重启 ssh服务
service sshd restart
```
