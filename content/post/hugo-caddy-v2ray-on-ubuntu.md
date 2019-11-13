---
title: Hugo Caddy V2ray on Ubuntu
author: Suan
date: 2019-11-10T02:09:16.066Z
lastmod: 2019-11-10T02:09:16.132Z
tags:
  - v2ray
  - Caddy
  - Hugo
---
## 前言

这个教程绝大部分代码你都可以复制粘贴，但是请仔细阅读代码里面的备注，#后面的文字。    

> 特别提醒请注意修改代码里面的参数！

## 必要条件

1. VPS 服务器 Ubuntu 18.04 系统
2. 域名一个
3. 域名已经绑定 VPS的 ip

## Hugo

1. 安装 Hugo

```bash
#使用 wget 下载 hugo
wget https://github.com/gohugoio/hugo/releases/download/v0.59.0/hugo_0.59.0_Linux-64bit.deb
#使用 dpkg 安装 .deb 文件
sudo dpkg -i hugo_0.59.0_Linux-64bit.deb
```

2. 创建网站存放目录

```bash
# 使用 mkdir命令，创建网站目录
mkdir -p /var/www
# 使用 cd命令，进入创建的目录
cd /var/www
# 新建站点，名字为3cho,你可以替换成自己的名字
hugo new site 3cho
```

3. 安装主题，主题可以去Hugo[官方主题库](https://themes.gohugo.io/)下载,本文以 [even](https://github.com/olOwOlo/hugo-theme-even) 主题为例

```bash
# 进入新建好的3cho目录
cd /var/www/3cho
# 安装主题（不同主题安装方式不同）
git clone https://github.com/olOwOlo/hugo-theme-even themes/even
#复制主题自带的 config.toml文件到当前根目录
cp themes/even/exampleSite/config.toml ./config.toml
```

4. 创建一篇测试新文章

```bash
hugo new post/hello.md
```

5. 生成博客静态博客

```bash
# 生成静态网页，包括草稿，生成好的内容在public目录中
hugo -D
```

## GitHub

1. 创新一个新的Github仓库
2. 初始化 GitHub,并上传全部 Hugo博客源码

```bash
#仓库名字repository以3cho为例
echo "# 3cho" >> README.md
git init
git add .
git commit -m "first commit"
git remote add origin git@github.com:替换成你的 GitHub用户名/3cho.git
git push -u origin master
```

## V2ray

1. 更新时间

```bash
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
date -R
```

2. 安装 V2ray

```bash
wget https://install.direct/go.sh
sudo bash go.sh
```

V2ray 控制命令,以后使用。

```bash
sudo systemctl start v2ray     # 启动v2ray
sudo systemctl status v2ray    # 查看v2ray状态
sudo systemctl stop v2ray      # 停止v2ray
sudo systemctl restart v2ray   # 重新启动v2ray
```

3. 编辑v2ray配置

```bash
nano /etc/v2ray/config.json
```

4. v2ray 配置内容

```json
{
  "inbounds": [
    {
      "port": 30123, //可以更改自己喜欢的端口号
      "listen":"127.0.0.1",//只监听 127.0.0.1，避免除本机外的机器探测到开放了 1$
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "0a10ad4e-0355-4435-8d17-4b75ab5677c4", //更改成自己的uuid
            "alterId": 64
          }
        ]
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/andykeywold" //任意关键字
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

粘贴完成后，你需要同时按 <kbd>ctrl</kbd>+<kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按<kbd>回车</kbd>确认保存。

5. 启动 V2ray并查看状态

```bash
sudo systemctl start v2ray    
sudo systemctl status v2ray
```

## Caddy

1. 安装 Caddy

```bash
curl https://getcaddy.com | bash -s personal
```

2. 配置目录权限

```bash
# 建立配置文件，更改文件所有权
mkdir /etc/caddy
touch /etc/caddy/Caddyfile
chown -R root:www-data /etc/caddy
# caddy自动获得的https证书存放位置
mkdir /etc/ssl/caddy
chown -R www-data:root /etc/ssl/caddy
# 私钥禁止其他用户访问
chmod 0770 /etc/ssl/caddy
# 建立日志目录，给与写入权限
mkdir /var/log/caddy
touch /var/log/caddy/access.log
chown -R www-data:root /var/log/caddy
chmod 0666 /var/log/caddy/access.log
```

3. 编辑 Caddy配置文件

```bash
nano /etc/caddy/Caddyfile
```

4. Caddy配置文件

```bash
3cho.cn
{
  gzip
  tls 123456212@mail.com
  log /var/log/caddy/access.log
  root /var/www/3cho/public
  proxy /andykeywold localhost:30123 {
    websocket
    header_upstream -Origin
  }
}
```

粘贴完成后，你需要同时按 <kbd>ctrl</kbd>+<kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按<kbd>回车</kbd>确认保存。

6. 创建 systemd配置文件，实现自启动

```bash
nano /etc/systemd/system/caddy.service
```

粘贴复制下面官方提供的配置

```
[Unit]
Description=Caddy HTTP/2 web server
Documentation=https://caddyserver.com/docs
After=network-online.target
Wants=network-online.target systemd-networkd-wait-online.service

[Service]
Restart=on-abnormal

; Do not allow the process to be restarted in a tight loop. If the
; process fails to start, something critical needs to be fixed.
StartLimitIntervalSec=14400
StartLimitBurst=10

; User and group the process will run as.
User=www-data
Group=www-data

; Letsencrypt-issued certificates will be written to this directory.
Environment=CADDYPATH=/etc/ssl/caddy

; Always set "-root" to something safe in case it gets forgotten in the Caddyfile.
ExecStart=/usr/local/bin/caddy -log stdout -log-timestamps=false -agree=true -conf=/etc/caddy/Caddyfile -root=/var/tmp
ExecReload=/bin/kill -USR1 $MAINPID

; Use graceful shutdown with a reasonable timeout
KillMode=mixed
KillSignal=SIGQUIT
TimeoutStopSec=5s

; Limit the number of file descriptors; see `man systemd.exec` for more limit settings.
LimitNOFILE=1048576
; Unmodified caddy is not expected to use more than that.
LimitNPROC=512

; Use private /tmp and /var/tmp, which are discarded after caddy stops.
PrivateTmp=true
; Use a minimal /dev (May bring additional security if switched to 'true', but it may not work on Raspberry Pi's or other devices, so it has been disabled in this dist.)
PrivateDevices=false
; Hide /home, /root, and /run/user. Nobody will steal your SSH-keys.
ProtectHome=true
; Make /usr, /boot, /etc and possibly some more folders read-only.
ProtectSystem=full
; … except /etc/ssl/caddy, because we want Letsencrypt-certificates there.
;   This merely retains r/w access rights, it does not add any new. Must still be writable on the host!
ReadWritePaths=/etc/ssl/caddy
ReadWriteDirectories=/etc/ssl/caddy

; The following additional security directives only work with systemd v229 or later.
; They further restrict privileges that can be gained by caddy. Uncomment if you like.
; Note that you may have to add capabilities required by any plugins in use.
;CapabilityBoundingSet=CAP_NET_BIND_SERVICE
;AmbientCapabilities=CAP_NET_BIND_SERVICE
;NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

粘贴完成后，你需要同时按 <kbd>ctrl</kbd>+<kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按<kbd>回车</kbd>确认保存。

Caddy 服务使用

```
systemctl enable caddy.service # 设置 caddy 服务自启动
systemctl start caddy.service  # 启动 caddy 服务
systemctl status caddy.service # 查看 caddy 状态
```

7. 启动 caddy服务

```bash
systemctl start caddy.service
```
完成！访问自己的域名试试吧! 

## 后话

如果你当正常博客使用，你还需要把下面这些坑填上。    

1. Hugo没有修改博客配置文件。
2. V2ray没有客户端配置教程。
3. Caddy启动方式比较原始，需要配置自动化。
4. 没写以后博客如何更新文章。
