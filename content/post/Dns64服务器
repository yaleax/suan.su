---
title: "Dns64服务器"
date: 2020-03-21T20:36:25+08:00
draft: true
tags:
  - dns64

---

### Dns64 server是什么

你有一台 ipv6的机器，想访问 ipv4的网站，Dns64 server就可以帮助你解决这个问题。路由图大概是这个样子

![](https://img.yaleax.com/u-1641462757-2111149660-fm-26-gp-0.jpg)

1.修改 dns服务器

```bash
nano /etc/resolv.conf
```

2.删除以前的nameserver ,使用下面这两个

```
nameserver 2001:67c:2b0::4 
nameserver 2001:67c:2b0::6
```

-----

3.保存

 粘贴完成后，你需要同时按 `ctrl`+`x`来退出,再输入`y`确认保存，再按`Enter`确认保存。

-----

[参考1] ：[https://wangdalao.com/2913.html](https://wangdalao.com/2913.html)

[参考2]：[http://www.trex.fi/2011/dns64.html](http://www.trex.fi/2011/dns64.html)

