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

经过一周的摸索和实践，终于把这个博客搭建成功了。简单介绍一下本博客所使用的技术。博客生成平台使用的是 [Hugo](https://gohugo.io/)，主题是 [Jane](https://github.com/xianmin/hugo-theme-jane),博客代码托管在 [GitHub](https://github.com/)，网站托管供应商是 [Netlify](https://www.netlify.com/)，博客后台管理平台是 [Netlify CMS](https://www.netlifycms.org/)，域名购买于 [RuTLD](https://ru-tld.ru/en/),DNS服务提供商是[CloudFlare](CloudFlare)，域名证书和 CDN网络加速也是由 [ClouFlare](https://www.cloudflare.com/) 。上面提到的这些服务，除了购买域名需要花钱，其他服务都是免费的，购买域名不是必要条件， Netlify 可以提供的二级域名。 

## 一、前置条件

1. 点击注册 [GitHub](https://github.com/join)
2. 点击注册 [Netlify](https://app.netlify.com/signup)
3. 点击注册 [CloudFlare](https://dash.cloudflare.com/sign-up)

## 二、Hugo 介绍

1. Hugo 是使用 Go 语言编写的，静态博客生成平台。使用Hugo平台，你可以快速的生成博客页面，因为是静态博客，所以访问者的载入速度是非常快的，静态博客对爬虫是很友好的，这样就利于被搜索引擎抓取网站。
2. 多平台支持，你可以在自己的电脑上搭建Hugo平台，然后把生成好的静态博客推送到 GitHub里。放心，主流的操作系统都支持安装 hugo。
3. Netlify 提供免费的静态博客托管服务和免费二级域名（类似<https://yia.netlify.com>），
4. GitHub提供免费代码托管服务，Hugo 生成的代码，会存在这里里。
5. Netlify CMS是一套开源的后台管理平台，支持 Markdown 语法，有了它，你可以以在线的方式更新博客了。如果你喜欢传统的 git 推送模式，最好也安装一个，因为它可以帮你在任何电子设备上，更新博客，修改错别字。

## 三、安装 Hugo

下面以 ubuntu 系统为例安装：

先去 [Hugo release](https://github.com/gohugoio/hugo/releases)查看 Hugo的最新版本，用最新版本号替代下面代码里面的0.59.0，你也可以直接复制下面的代码，进行下载和安装。

```
#使用 wget 下载 hugowget https://github.com/gohugoio/hugo/releases/download/v0.59.0/hugo_0.59.0_Linux-64bit.deb#使用 dpkg 安装 .deb 文件sudo dpkg -i hugo_0.59.0_Linux-64bit.deb
```

创建网站存放目录

```
# 使用 mkdir命令，创建网站目录
```

```
mkdir -p /var/www
```

```
# 使用 cd命令，进入创建的目录
```

```
cd /var/www
```

```
# 新建站点，名字为3cho,你可以替换成自己的名字
```

```
hugo new site 3cho
```
