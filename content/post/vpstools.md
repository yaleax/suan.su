---
title: vpstools
date: 2022-06-22T06:18:56.241Z
lastmod: 2022-06-22T06:18:56.273Z
---


## 一键命令 One click command

```
apt -o Acquire::AllowInsecureRepositories=true -o Acquire::AllowDowngradeToInsecureRepositories=true update && apt-get install sudo curl screen -y && curl -LO https://raw.githubusercontent.com/johnrosen1/vpstoolbox/master/vps.sh && sudo screen -U bash vps.sh
```

> 仅支援 **Debian/Ubuntu** 系统。

![](https://img.jze.us/file/j20usw/Screen-Shot-2022-06-22-14-20-07.99)
