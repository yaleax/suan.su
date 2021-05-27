---
title: 甲骨文ARM Ubuntu一键DD脚本
date: 2021-05-27T06:49:47.559Z
lastmod: 2021-05-27T06:49:47.601Z
---
1.新建实例时选的 ubuntu 20.4，非 mini 版。

2.下载脚本

```
curl -fLO https://raw.githubusercontent.com/bohanyang/debi/master/debi.sh
```

3.执行脚本

```
sudo ./debi.sh --architecture arm64 --user root --password password
```

4.设置默认root的密码为password，登陆成功之后记得自己输入passwd修改密码！！！

5.重启电脑，

```
sudo shutdown -r now
```

6.等一会使用命令登录

```
ssh root@1.2.3.4
```
﻿来源：
[保姆级教程！甲骨文ARM DD成Debian10并升级内核成5.10-美国VPS综合讨论-全球主机交流论坛 - Powered by Discuz! (hostloc.com)](https://hostloc.com/thread-849672-1-1.html)
