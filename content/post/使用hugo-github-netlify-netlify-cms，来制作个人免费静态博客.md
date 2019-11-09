---
title: 使用Hugo +GitHub + Netlify+Netlify CMS，来制作个人免费静态博客
description: 使用Hugo +GitHub + Netlify+Netlify CMS，来制作个人免费静态博客
date: 2019-11-09T06:46:37.869Z
lastmod: 2019-11-09T06:46:37.895Z
tags:
  - hugo
  - 博客
weight: ''
---
## 零、起源

经过一周的摸索和实践，终于把这个博客搭建成功了。简单介绍一下博客所使用的技术支持。博客生成平台使用的是 [Hugo](https://gohugo.io/)，主题是 [Jane](https://github.com/xianmin/hugo-theme-jane),博客代码托管在 [GitHub](https://github.com/)，网站托管供应商是 [Netlify](https://www.netlify.com/)，博客后台管理平台是 [Netlify CMS](https://www.netlifycms.org/)，域名购买于 [RuTLD](https://ru-tld.ru/en/),DNS服务提供商是[CloudFlare](CloudFlare)，域名证书和 CDN网络加速也是由 [ClouFlare](https://www.cloudflare.com/) 。上面提到的这些服务，除了购买域名需要花钱，其他都是免费的，购买域名不是必须的， Netlify 可以提供的二级域名，后面会提到。 

## 一、前置条件

1. 注册 [GitHub](https://github.com/)
2. 注册 [Netlify](https://www.netlifycms.org/)
3. 注册[CloudFlare](CloudFlare)

## 二、Hugo 介绍

1. Hugo 是使用 Go 语言编写的，静态博客生成平台。使用Hugo平台，你可以快速的生成博客页面，因为是静态博客，所以访问者的载入速度是非常快的，静态博客对爬虫是很友好的，这样就利于被搜索引擎抓取网站。
2. 多平台支持，你可以在自己的电脑上搭建Hugo平台，然后把生成好的静态博客推送到 GitHub里。放心，主流的操作系统都支持安装 hugo。
3. Netlify 提供免费的静态博客托管服务和免费二级域名（类似<https://yia.netlify.com>），GitHub提供免费代码托管服务，Hugo 生成的文件，都会存在 GitHub里。
4. Netlify CMS是一套开源的后台管理平台，支持 Markdown 语法，有了它，你可以以在线的方式更新博客了。如果你喜欢传统的 git 推送模式，最好也安装一个，因为，它可以帮你在任何电子设备上，更新博客，修改错别字。
   5.题外话，在多天的

## 三、安装 Hugo

在 Mac和 Linux 系统上，安装 Hugo 是很简单的事情，官方文档只有一句：

brew install hugo

如果你没有 brew 工具，你可以安装 brew 工具，对于 Mac电脑也是一句：

```
/usr/bin/ruby -e "$(curl -fsSLhttps://raw.githubusercontent.com/Homebrew/install/master/install)"
```

对于 Linxu
