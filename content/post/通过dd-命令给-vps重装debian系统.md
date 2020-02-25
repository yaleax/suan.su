---
title: 通过dd 命令给 VPS重装debian系统
date: 2020-02-25T02:43:40.419Z
lastmod: 2020-02-25T02:43:41.956Z
---
### 前言
当你想在 vps 里面重新安装一个干净的系统的时候，你就可以用下面的方法来执行。注意看说明，这个不是一键脚本，需要修改参数。

### 1.装依赖组件
```bash
apt-get install -y xz-utils openssl gawk file
```
### 2.查看机子原来ip
```bash
cat /etc/network/interfaces
```
### 3.修改好ip整段丢进终端运行 

```bash
wget --no-check-certificate -c http://moeclub.org/attachment/LinuxShell/InstallNET.sh
sed -i 's/8.8.8.8/1.1.1.1/g' InstallNET.sh
bash InstallNET.sh -d 9 -v amd64 -a \
-p admin \
--mirror "http://deb.debian.org/debian/debian" \
--ip-addr 10.10.10.10 \
--ip-mask 255.255.255.0 \
--ip-gate 10.10.10.1
```
{{% notice info 备注 %}}  
第1行：下载脚本  
第2行：改dns为1.1.1.1  
第3行：想安装 debian几，数字就改成几。  
第4行：改root预设密码为admin  
第5行：改镜像源  
第6-8行：填写2查看的原 IP 地址    
{{% /notice %}}
