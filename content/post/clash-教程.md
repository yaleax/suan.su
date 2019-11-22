---
title: Clash 教程
date: 2019-11-18T08:37:54.156Z
lastmod: 2019-11-18T08:37:55.541Z
tags:
  - Clash
---
## 一、初衷
## 二、必要条件
## 三、安装Clash到 Linux 系统上

### 1. 下载并安装Clash

```bash
进入当前用户根目录
cd ~
#下载二进制文件
wget https://github.com/Dreamacro/clash/releases/download/v0.16.0/clash-linux-amd64-v0.16.0.gz 
#使用 gzip 解压
gzip -d clash-linux-amd64-v0.16.0.gz 
#移动到bin
mv clash-linux-amd64-v0.16.0 /usr/local/bin/clash 
#添加执行权限
chmod +x /usr/local/bin/clash 
```

### 2.配置Clash启动服务

```bash
nano /etc/systemd/system/clash.service
```
把下面的信息复制进去
{{% notice info clash.service配置 %}}

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

{{% /notice %}}


### 3.设置 Clash 开机自启

```bash
systemctl daemon-reload
systemctl enable clash
```

{{% notice tip 其他功能%}}

service clash start

{{% /notice %}}

### 4.下载 dashboard 控制面板

```bash
# 下载 dashboard
wget https://github.com/haishanh/yacd/archive/gh-pages.zip
# 用 unzip解压缩 
unzip gh-pages.zip
# 把文件改名层 dashboard
mv yacd-gh-pages/ dashboard/
```



### 5.配置 Clash 文件

```bash
# 进入配置文件目录
cd ~/.config/
# 创建 Clash 目录
mkdir clash
# 编辑 Clash 配置文件config.yaml
nano config.yaml

```

{{% notice info config.yaml配置 %}}

配置写在这里

{{% /notice %}}

### 6.启动前

配置文件里面的加密是重点

### 7.启动 Clash

```bash
service clash start
```

### 8.:tada: 完成

访问http://serverip:9090/ui/ 试试吧

## 四、通过V2ray访问已安装Clash的服务器



------
[参考1]<https://breakertt.moe/2019/08/20/clash_gateway/index.html>    
[参考2]<https://qust.me/post/678ffe99.html>
