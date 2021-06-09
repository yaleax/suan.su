---
title: 给vps套上warp来解锁奈飞
date: 2021-03-24T05:40:52.544Z
lastmod: 2021-03-24T05:40:52.674Z
---

本教程只是用于能够修改内核的KVM产品，或者内核高于5.6的无法修改内核的产品。unamer -r 可以查看自己的内核。下面所有命令只在Debian/Ubuntu环境下工作。

## 一、升级内核

### 1.1选择2升级内核，5.6以上的内核自带wireguard

```bash
bash <(wget --no-check-certificate -qO- https://raw.githubusercontent.com/jacyl4/de_GWD/main/server)
```

## 二、Wireguard

### 2.1 安装Wireguard
```bash
apt install wireguard
```

## 三、Warp

### 3.1安装Wgcf (warp)

```bash
curl -fsSL git.io/wgcf.sh | sudo bash
```

### 3.2 自动生成warp账号

```bash
wgcf register
```

### 3.3 自动生成Wireguard配置

```bash
wgcf generate
```

### 3.4 修改Wireguard配置

```bash
nano wgcf-profile.conf
```

### 3.5 配置内容(给ipv4增加IPv6地址)

```
[Interface]
PrivateKey = mIGbBq8ZLrsgnDM0fwjfd7Qu4cIbjf7ZhvnX3KE8N2o=
Address = 172.16.0.2/32
Address = fd01:5ca1:ab1e:881c:570:1058:47ee:23f1/128
DNS = 9.9.9.10,8.8.8.8,1.1.1.1
MTU = 1280
[Peer]
PublicKey = bmXOC+F1FxEMF9dyiK2H5/1SUtzH0JuVo51h2wPfgyo=
AllowedIPs = ::/0
Endpoint = 162.159.192.1:2408
```

```
[Interface]
PrivateKey = GDA8jNe6kr2jXf9DjBtpobXjcFDO8fpa/IZWr3sYpVY=
Address = 172.16.0.2/32
Address = fd01:5ca1:ab1e:8b43:4d28:9678:4fbb:1f61/128
DNS = 2001:4860:4860::8888,2001:4860:4860::8844,8.8.8.8,8.8.4.4
MTU = 1280
[Peer]
PublicKey = bmXOC+F1FxEMF9dyiK2H5/1SUtzH0JuVo51h2wPfgyo=
AllowedIPs = 0.0.0.0/0
Endpoint = [2606:4700:d0::a29f:c001]:2408
```﻿

### 3.6 移动配置到Wireguard目录

```bash
sudo cp wgcf-profile.conf /etc/wireguard/wgcf.conf
```

### 3.7 启动Wireguard

```bash
sudo wg-quick up wgcf
```

### 3.8 测试ipv6 是否生效

```bash
# IPv4 Only VPS
curl -6 ip.p3terx.com
# IPv6 Only VPS
curl -4 ip.p3terx.com
```

### 3.9 关闭Wirguard 

```bash
sudo wg-quick down wgcf
```

### 3.10 启动开机自动启动

```bash
# 启用守护进程
sudo systemctl start wg-quick@wgcf
# 设置开机启动
sudo systemctl enable wg-quick@wgcf
```

## 四、代理设置

### 4.1 IPv6 优先

```bash
grep -qE '^[ ]*label[ ]*2002::/16[ ]*2' /etc/gai.conf || echo 'label 2002::/16   2' | sudo tee -a /etc/gai.conf
```

### 4.2 通过代理设置IPv6优先

```json
"outbounds": [
    {
        "sendThrough": "::",
        "protocol": "freedom",
        "settings": {
            "domainStrategy": "UseIP"
        }
    }
]
```

## 五、一些常用命令

```bash
nano /opt/de_GWD/vtrui/config.json
systemctl restart vtrui
nano /etc/wireguard/wgcf.conf
```



## 六、引用

[在VPS上部署WARP来进行流量加密并解锁Netflix和Google验证码（无损实现IPv4/IPv6双栈） | Hiram Home](https://hiram.wang/cloudflare-wrap-vps/)

[使用 Cloudflare WARP 给 VPS 服务器免费添加 IPv4 或 IPv6 网络支持 - P3TERX ZONE](https://p3terx.com/archives/use-cloudflare-warp-to-add-extra-ipv4-or-ipv6-network-support-to-vps-servers-for-free.html)

[Debian Linux VPS 服务器 WireGuard 安装教程 - P3TERX ZONE](https://p3terx.com/archives/debian-linux-vps-server-wireguard-installation-tutorial.html)

[关于v2ray在搭建了cloudflare warp的机器上无法按照系统路由流量的解决方法 – LOUKKY的博客](https://loukky.com/archives/1507)

