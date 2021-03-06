---
title: Hugo Caddy V2ray on Ubuntu and filebrowser
author: Suan
date: 2019-11-10T02:09:16.066Z
lastmod: 2019-11-10T02:09:16.132Z
tags:
  - v2ray
  - Caddy
  - Hugo
weight: ''
---
## 一、前言

这个教程绝大部分内容你都可以复制粘贴，但是请仔细阅读代码框里面的备注，也就是`#`后面的文字。    

{{% notice warning 特别提醒 %}}
:u7981:必须修改代码里面的参数，否则不可能成功！有问题就留言吧，祝你好运。
{{% /notice %}}

## 二、必要条件
### 1.  Ubuntu服务器
### 2. 域名
### 3. 域名绑定服务器的IP 地址
## 三、Hugo

### 1.安装 Hugo  
先去 [Hugo release](https://github.com/gohugoio/hugo/releases)查看 Hugo的最新版本，用最新版本号替代下面代码里面的0.59.1，你也可以直接复制下面的代码，进行下载和安装。
```bash
#使用 wget 下载 hugo
wget https://github.com/gohugoio/hugo/releases/download/v0.59.1/hugo_0.59.1_Linux-64bit.deb
#使用 dpkg 安装 .deb 文件
sudo dpkg -i hugo_0.59.1_Linux-64bit.deb
```

### 2.创建网站存放目录

```bash
# 使用 mkdir命令，创建网站目录
mkdir -p /var/www
# 使用 cd命令，进入创建的目录
cd /var/www
# 新建站点，名字为3cho,你可以替换成自己的名字
hugo new site suan
```

### 3.安装主题
{{% notice tip 提示 %}}  
如果没有安装 git先安装
{{% /notice %}}
### 3.1安装 git
```bash
apt-get update
apt-get install git
```
###3.2 安装主题
主题可以去Hugo[官方主题库](https://themes.gohugo.io/)下载,本文以 [even](https://github.com/olOwOlo/hugo-theme-even) 主题为例
```bash
# 进入新建好的suan目录
cd /var/www/suan
# 安装主题（不同主题安装方式不同）
git clone https://github.com/olOwOlo/hugo-theme-even themes/even
#复制主题自带的 config.toml文件到当前根目录
cp themes/even/exampleSite/config.toml ./config.toml
```

### 4.创建一篇测试新文章

```bash
hugo new post/hello.md
```

### 5.生成博客静态博客

```bash
# 生成静态网页，包括草稿，生成好的内容在public目录中
hugo -D
```

## 四、GitHub
{{% notice tip 提示 %}}  
如果你以后不想通过 github管理文章，这里可以跳过。
{{% /notice %}}
### 1.新建一个Github仓库
输完仓库名字后，按 <kbd>enter</kbd> 。
![](https://img.suan.su/Screen-Recording-2019-11-17-20-13-44.gif)
### 2.初始化 GitHub
这些命令完成后，会上传全部Hugo博客源码到 GitHub
```bash
#仓库名字repository以hellotest为例
echo "# hellotest" >> README.md
git init
git add .
git commit -m "first commit"
git remote add origin git@github.com:替换成你的GitHub用户名/hellotest.git
git push -u origin master
```

## 五、V2ray

### 1.更新为上海时间

```bash
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
date -R
```

### 2.安装 V2ray

```bash
wget https://install.direct/go.sh
sudo bash go.sh
```

### 3.编辑v2ray配置

```bash
rm /etc/v2ray/config.json
nano /etc/v2ray/config.json
```

### 4.v2ray 配置内容

```json
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
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

{{% notice tip 提示 %}}  
修改完成后，你需要同时按 <kbd>ctrl</kbd> + <kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按 <kbd>enter</kbd> 保存。  
{{% /notice %}}

### 5.V2ray 控制命令

{{% notice info 命令信息 %}}  
sudo systemctl start v2ray     # 启动v2ray  
sudo systemctl status v2ray    # 查看v2ray状态  
sudo systemctl stop v2ray      # 停止v2ray  
sudo systemctl restart v2ray   # 重新启动v2ray
{{% /notice %}}

### 6.启动 V2ray并查看状态

```bash
sudo systemctl start v2ray    
sudo systemctl status v2ray
```
### 7.暂停 V2ray

```bash
sudo systemctl stop v2ray
```
## 六、Filebrowser

### 1.安装 Filebrowser

```bash
curl -fsSL https://filebrowser.xyz/get.sh | bash
```

安装目录：`/usr/local/bin/filebrowser`

### 2.创建 Filebrowser配置文件

```bash
mkdir /etc/filebrowser
mkdir /etc/filebrowser/filebrowser
nano /etc/filebrowser/filebrowser.json
```
### 3.Filebrowser.json配置信息
把这些信息复制进去
```json
{
  "port": 18888,
  "baseURL": "/admin",
  "address": "0.0.0.0",
  "log": "stdout",
  "database": "/var/www/suan/database.db",
  "root": "/var/www"
}
```

### 4.设置Filebrowser系统服务文件

```bash
nano /etc/systemd/system/filebrowser.service
```

### 5.filebrowser.service配置文件

```bash
[Unit]
Description=File Browser
After=network.target

[Service]
ExecStart=/usr/local/bin/filebrowser -c /etc/filebrowser/filebrowser.json

[Install]
WantedBy=multi-user.target
```

### 6.重新载入systemctl

```bash
systemctl daemon-reload
```

### 7.启动 filebrowser
```bash
systemctl start filebrowser

```

{{% notice info 命令信息 %}}  

状态：systemctl status filebrowser  
启动：systemctl start filebrowser  
停止：systemctl stop filebrowser  
重启：systemctl restart filebrowser  
{{% /notice %}}
### 8.登录地址  
http://你的ip:1888/admin   
帐号:`admin`  
密码:`admin`  
### 9.启动 https    
启动 https后面会介绍，利用 Caddy 反代,添加一句，后面 Caddy 配置文件我会加进去。  
`proxy /admin 127.0.0.1:18888 \\反代 filebreowser`


## 七、Caddy

### 1.安装 Caddy

```bash
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/caddy_install.sh && chmod +x caddy_install.sh && bash caddy_install.sh install http.filemanager http.git
#备用地址
wget -N --no-check-certificate https://www.moerats.com/usr/shell/Caddy/caddy_install.sh && chmod +x caddy_install.sh && bash caddy_install.sh~
```

### 2.设置 Cadddy所需要的目录
```bash
# 建立配置文件，更改文件所有权
mkdir /usr/local/caddy/
touch /usr/local/caddy/Caddyfile
chown -R root:www-data /usr/local/caddy/
# 建立日志目录，给与写入权限
mkdir /var/log/caddy
touch /var/log/caddy/access.log
chown -R www-data:root /var/log/caddy
chmod 0666 /var/log/caddy/access.log
```
### 3.编辑 Caddy配置文件

```bash
nano /usr/local/caddy/Caddyfile
```
### 4.Caddy配置文件

```bash
suan.su
{
  gzip
  tls 123456212@mail.com
  log /var/log/caddy/access.log
  root /var/www/suan/public
  proxy /admin 127.0.0.1:18888 \\反代 filebreowser
  proxy /ray30123 localhost:30123 {
    websocket
    header_upstream -Origin
  }
}
```

{{% notice tip 提示 %}}  
修改完成后，你需要同时按 <kbd>ctrl</kbd> + <kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按 <kbd>enter</kbd> 保存。  
{{% /notice %}}

### 5.Caddy文件说明
 
安装目录：`/usr/local/caddy/`  
Caddy配置文件位置：`/usr/local/caddy/Caddyfile`  
Caddy自动申请SSL证书位置：`/.caddy/acme/acme-v02.api.letsencrypt.org/sites/xxx.xxx(域名)/`  



### 6.Caddy 控制命令
{{% notice info 命令信息 %}}  
启动：/etc/init.d/caddy start  
停止：/etc/init.d/caddy stop  
重启：/etc/init.d/caddy restart  
查看状态：/etc/init.d/caddy status  
查看Caddy启动日志：tail -f /tmp/caddy.log  
{{% /notice %}}

### 7.启动 Caddy服务并查询状态

```bash
/etc/init.d/caddy start
sudo systemctl start v2ray    
tail -f /tmp/caddy.log 
```
 :tada: 完成！访问自己的域名试试吧!  

### 7.1 安装 bbrplus
```bash
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh"
chmod +x tcp.sh
./tcp.sh
```
## 八、后话

如果你当正常博客使用，你还需要把下面这些坑填上。    

1. [ ] 修改Hugo博客配置文件。  
2. [ ] V2ray客户端配置教程。  
3. [x] Caddy自动化配置。
4. [ ] 使用GitHub更新博客文章。
