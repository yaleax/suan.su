---
title: 使用Hugo +GitHub + Netlify+Netlify CMS，来制作个人免费静态博客
description: 使用Hugo +GitHub + Netlify+Netlify CMS，来制作个人免费静态博客
date: 2019-11-09T06:46:37.869Z
lastmod: 2019-11-09T06:46:37.895Z
tags:
  - hugo
  - 博客
weight: 1
---
## 一、Hugo 介绍

1. Hugo 是使用 Go 语言编写的，静态博客生成平台。使用Hugo平台，你可以快速的生成博客页面，因为是静态博客，所以访问者的载入速度也是非常快的，静态博客对爬虫是很友好的，利于被搜索引擎抓取网站。
2. 多平台支持，你可以在自己的电脑上搭建 Hugo平台，然后把生成好的静态博客推送到 GitHub上面。下面列举的电脑操作系统，都支持安装 Hugo。
3. Netlify 提供免费的静态博客托管服务和免费二级域名（类似https://yia.netlify.com），GitHub提供免费代码托管服务，Hugo 生成的文件，都会存到 GitHub上面。
4. 使用Linux 服务器搭建 Hugo+Caddy+V2ray+CDN(本文不介绍)

* Caddy 是 http 服务器
* V2ray是网络代理工具
* CND是分布式网络加速服务，服务商是 cloudflare。



二、安装 Hugo

在 Mac和 Linux 系统上，安装 Hugo 是很简单的事情，官方文档只有一句：

brew install hugo

如果你没有 brew 工具，你可以安装 brew 工具，对于 Mac电脑也是一句：

```
/usr/bin/ruby -e "$(curl -fsSLhttps://raw.githubusercontent.com/Homebrew/install/master/install)"
```

对于 Linxu
