---
title: 通过dd 命令给 VPS重装debian 11 系统
date: 2020-02-25T02:43:00.000Z
lastmod: 2021-04-09T01:10:00.000Z
comment: false
reward: false
weight: 1
---
## 前言

当你想在 vps 里面重新安装一个干净的系统的时候，你就可以用下面的方法来执行。注意看说明，这个不是一键脚本，需要修改参数。

## 一键傻瓜式安装

### 1.示例代码

下面这个是安装 Debian11,密码是: `MoeClub.org`

```bash
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -d 11 -v 64 -a
```

参照下面的提示,可以安装其他系统和版本

```bash
Usage:
        bash InstallNET.sh      -d/--debian [dist-name]
                                -u/--ubuntu [dist-name]
                                -c/--centos [dist-version]
                                -v/--ver [32/i386|64/amd64]
                                --ip-addr/--ip-gate/--ip-mask
                                -apt/-yum/--mirror
                                -dd/--image
                                -a/-m

# dist-name: 发行版本代号
# dist-version: 发行版本号
# -apt/-yum/--mirror : 使用定义镜像
# -a/-m : 询问是否能进入VNC自行操作. -a 为不提示(一般用于全自动安装), -m 为提示.
```





```
url -fLO https://raw.githubusercontent.com/bohanyang/debi/master/debi.sh && chmod a+rx debi.sh

sudo ./debi.sh --cdn --network-console --ethx --bbr --user root --password <新系统用户密码>

```

* 



### 1.1 整合代码包含windows，

Linux默认密码：`MoeClub.org`  or  `cxthhhhh.com` 

```
##镜像文件在OneDrive
wget -N --no-check-certificate https://raw.githubusercontent.com/veip007/dd/master/dd-od.sh && chmod +x dd-od.sh && ./dd-od.sh

##镜像文件在GoogleDrive
wget -N --no-check-certificate https://raw.githubusercontent.com/veip007/dd/master/dd-gd.sh && chmod +x dd-gd.sh && ./dd-gd.sh
```

### 1.2 Windows7 精简包 账户：Administrator 密码：nat.ee

```bash
wget --no-check-certificate -O InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && bash InstallNET.sh -dd 'http://a.iplc.best/natee/lite/win7-ent-sp1-x64-cn/win7-ent-sp1-x64-cn.vhd.gz'
```

1. ### 1.2 激活

```bash
slmgr.vbs -upk
slmgr -skms skms.netnr.eu.org
slmgr -ipk 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH 
slmgr -ato
slmgr.vbs -ato
slmgr.vbs -dlv
```

## 自定义 IP 安装

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

第1行：下载萌咖大佬的脚本

第2行：改dns为1.1.1.1

第3行：想安装 debian几，就把数字9改成几。

第4行：改root预设密码为admin

第5行：改镜像源

第6-8行：填写2查看的原 IP 地址

\
{{% /notice %}}

- - -

\[参考1]：[https://github.com/jacyl4/de_GWD](https://github.com/jacyl4/de_GWD/wiki/%E9%87%8D%E8%A3%85vps-debian-%E9%80%9A%E8%BF%87dd-%E5%91%BD%E4%BB%A4%E8%A1%8C-%E6%96%B9%E5%BC%8F-%E6%AD%A3%E7%A1%AE%E7%94%A8%E6%B3%95)

\[参考2]：<https://moeclub.org/2018/04/03/603/>

\[参考3]：<http://a.iplc.best/natee/%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85.txt>