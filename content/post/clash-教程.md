---
title: Clash 教程
date: 2019-11-18T08:37:54.156Z
lastmod: 2019-11-18T08:37:55.541Z
tags:
  - Clash
---
## 一、安装

### 1. 下载并安装Clash

```bash
进入当前用户根目录
cd ~
#下载二进制文件
wget https://github.com/Dreamacro/clash/releases/download/v0.16.0/clash-linux-amd64-v0.16.0.gz 
#使用 gzip 解压
gzip -d clash-linux-amd64-v0.15.0.gz 
#移动到bin
mv clash-linux-amd64-v0.15.0 /usr/local/bin/clash 
#添加执行权限
chmod +x /usr/local/bin/clash 
```

### 2.配置Clash启动服务

```bash
[Unit]
Description=clash service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/clash
Restart=on-failure # or always, on-abort, etc

[Install]
WantedBy=multi-user.target
```

### 3.设置 Clash 开机自启

```bash
systemctl daemon-reload
systemctl enable clash
```

{{% notice tip 其他功能%}}

```bash
service clash start
```

{{% /notice %}}

### 4.下载 dashborad 控制面板

```bash
wget https://github.com/haishanh/yacd/archive/gh-pages.zip
unzip gh-pages.zip
mv yacd-gh-pages/ dashboard/
```



### 5.配置 Clash 文件

```bash
cd ~/.config/
# 创建 Clash 目录
mkdir clash
nano config.yaml

```

{{% notice info %}}

配置在这里

{{% /notice %}}

### 6.启动 Clash

```bash
service clash start
```

### 7.:tada: 完成

访问http://serverip:9090/ui/ 试试吧



