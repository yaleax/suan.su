---
title: VPS 上面安装 emby 服务器并连接 google drvie
date: 2019-11-30T11:24:02.183Z
lastmod: 2019-11-30T11:24:03.291Z
---

## 一、前言
纯属抄袭！在去[原创](http://dxz.plus/index.php/2019/11/03/10.html)之前，可以先把 vps 连接上 google drive，教程在[这里](https://suan.su/post/vps%E8%BF%9E%E6%8E%A5-google-drive/)。
## 二、emby
### 1.安装
```bash
docker run --name=emby -d -v /root/emby/config:/config     -v /root/rclone:/clone     -p 8096:8096  -p 8920:8920  -e UID=1000  -e GID=100  -e GIDLIST=100 --restart unless-stopped  emby/embyserver:latest
```
## 三、访问
http://youip:8096
