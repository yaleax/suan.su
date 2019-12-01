---
title: '在 ubuntu服务器上安装Clash '
date: 2019-11-18T08:37:54.156Z
lastmod: 2019-11-18T08:37:55.541Z
tags:
  - Clash
---
## 一、前言
这是一个媲美 Surge 的代理客户端，免费、简洁的界面，支持 SS、V2Ray、SOCKS5 协议、支持规则分流和屏蔽广告（类似 Surge 的规则），支持托管订阅。
## 二、必要条件
### 1. linux服务器一台
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
service clash start    # 启动    
service clash stop     # 停止  
service clash restart  # 重启  
service clash status   # 状态  
{{% /notice %}}

### 4.下载 dashboard 控制面板

```bash
# 创建 .config目录
mkdir .config 
# 创建 clash 目录
mkdir ~/.config/clash
# 下载 dashboard
wget https://github.com/haishanh/yacd/archive/gh-pages.zip
# 用 unzip解压缩 
unzip gh-pages.zip
# 把文件改名层 dashboard
mv yacd-gh-pages/ ~/.config/clash/dashboard/
```



### 5.配置 Clash 文件

```bash
# 进入配置文件目录
cd ~/.config/
# 编辑 Clash 配置文件config.yaml
nano config.yaml

```

{{% notice info config.yaml配置 %}}

[去 GitHub 自己复制吧]<https://github.com/ConnersHua/Profiles/edit/master/Clash/Pro.yaml>

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

v2ray配置如下
```bash
{
  "inbounds": [
    {
      "port": 30123,
      "listen":"127.0.0.1", //只监听 127.0.0.1，避免除本机外的机器探测到开放了 30123 端口
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "b831381d-6324-4d53-ad4f-8cda48b30811",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/ray30123"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "socks",
      "settings": {
        "settings": {
        "servers": [{
        "address": "127.0.0.1",
        "port": 7891,
        "auth": "noauth"
      }]
     }
    }
  ]
}
```    
### [在线 json 编辑器](http://jsoneditoronline.org/)

## 五、关闭端口

```bash
#打开端口
iptables -A INPUT -p   $port   -j ACCEPT
#关闭端口  
iptables -A INPUT -p $port   -j DROP

```

------
[参考1]<https://breakertt.moe/2019/08/20/clash_gateway/index.html>    
[参考2]<https://qust.me/post/678ffe99.html>
