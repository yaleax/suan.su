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

经过一周的摸索和实践，终于把这个博客搭建成功了。简单介绍一下本博客所使用的技术。博客生成平台使用的是 [Hugo](https://gohugo.io/)，主题是 [Jane](https://github.com/xianmin/hugo-theme-jane),博客代码托管在 [GitHub](https://github.com/)，网站托管供应商是 [Netlify](https://www.netlify.com/)，博客后台管理平台是 [Netlify CMS](https://www.netlifycms.org/)，域名购买于 [RuTLD](https://ru-tld.ru/en/),DNS服务提供商是[CloudFlare](CloudFlare)，域名证书和 CDN网络加速也是由 [CloudFlare](https://www.cloudflare.com/) 。上面提到的这些服务，除了购买域名需要花钱，其他服务都是免费的，购买域名不是必要条件， Netlify 可以提供的二级域名。 

## 一、前置条件

1. 点击注册 [GitHub](https://github.com/join)
2. 点击注册 [Netlify](https://app.netlify.com/signup)
3. 点击注册 [CloudFlare](https://dash.cloudflare.com/sign-up)
4. 一台可以访问网络的计算机

## 二、Hugo 介绍

1. Hugo 是使用 Go 语言编写的，静态博客生成平台。使用Hugo平台，你可以快速的生成博客页面，因为是静态博客，所以访问者的载入速度是非常快的，静态博客对爬虫是很友好的，这样就利于被搜索引擎抓取网站。
2. 多平台支持，你可以在自己的电脑上搭建Hugo平台，然后把生成好的静态博客推送到 GitHub里。放心，主流的操作系统都支持安装 hugo。
3. Netlify 提供免费的静态博客托管服务和免费二级域名（类似<https://yia.netlify.com>），
4. GitHub提供免费代码托管服务，Hugo 生成的代码，会存在这里里。
5. Netlify CMS是一套开源的后台管理平台，支持 Markdown 语法，有了它，你可以以在线的方式更新博客了。如果你喜欢传统的 git 推送模式，最好也安装一个，因为它可以帮你在任何电子设备上，更新博客，修改错别字。

## 三、安装 Hugo

1.Ubuntu系统：

先去 [Hugo release](https://github.com/gohugoio/hugo/releases)查看 Hugo的最新版本，用最新版本号替代下面代码里面的0.59.0，你也可以直接复制下面的代码，进行下载和安装。

```bash
#使用 wget 下载 hugo
wget https://github.com/gohugoio/hugo/releases/download/v0.59.0/hugo_0.59.0_Linux-64bit.deb
#使用 dpkg 安装 .deb 文件
sudo dpkg -i hugo_0.59.0_Linux-64bit.deb
```
2.MacOS 系统：

```bash
brew install hugo
```
如果你没有[brew](https://brew.sh/)包工具，你可以用下面的命令安装brew包工具

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 四、使用 Hugo创建静态博客 

1.创建网站存放目录

```bash
# 使用 mkdir命令，创建网站目录
mkdir -p /var/www
# 使用 cd命令，进入创建的目录
cd /var/www
# 新建站点，名字以3cho为例,你可以替换成自己的名字
hugo new site 3cho
```

2.安装主题，主题可以去Hugo[官方主题库](https://themes.gohugo.io/)下载

```bash
# 进入新建好的3cho目录
cd /var/www/3cho
# 安装主题（不同主题安装方式不同）
git clone https://github.com/olOwOlo/hugo-theme-even themes/even
#复制主题自带的 config.toml文件到当前根目录
cp themes/even/exampleSite/config.toml ./config.toml
```
3.创建一篇测试新文章
```bash
#生成一篇新文章
hugo new post/hello.md
4.生成博客静态博客
# 生成静态网页，包括草稿，生成好的内容在public目录中
hugo -D
```
5.启动本地博客服务器
```bash
hugo server
```
点击访问你的博客[http://localhost:1313](http://localhost:1313)

到这里，博客搭建工作已经完成了，接下来是把这个博客，部署到网络上，这样其他人才可以访问你的博客了。

6关闭本地博客服务器

键盘同时按Control建和c建，关闭本地博客服务器

## 五、Create a new repository 创新一个新的 repository在 GitHub

[点击查看官方教程](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository)，你不需要看文字，跟着图走就行。repository名字是什么很重要，下面我们要用它来替换3cho。

1.初始化 GitHub，初始化意思是初次建立本地目录和 GitHub远程仓库的连接

```bash
#仓库名字repository以3cho为例
echo "# 3cho" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:替换成你的 GitHub用户名/3cho.git
git push -u origin master
```

## 五、Netlify 

1.删除主题下面的.git文件，主题以 even为例

```Bash
cd /var/www/3cho/themes/even
rm -rf .git
```
2.创建 Netlify 需要的配置文件

```Bash
nano netlify.toml
```
复制下面的文件粘贴进去，Hugo version后面的数字替换成，你安装的版本。不明白什么意思，可以不改。
```toml
[build]
publish = "public"
command = "hugo --gc --minify"

[context.production.environment]
HUGO_VERSION = "0.59.1"
HUGO_ENV = "production"
HUGO_ENABLEGITINFO = "true"

[context.split1]
command = "hugo --gc --minify --enableGitInfo"

[context.split1.environment]
HUGO_VERSION = "0.59.1"
HUGO_ENV = "production"

[context.deploy-preview]
command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"

[context.deploy-preview.environment]
HUGO_VERSION = "0.59.1"

[context.branch-deploy]
command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"

[context.branch-deploy.environment]
HUGO_VERSION = "0.59.1"

[context.next.environment]
HUGO_ENABLEGITINFO = "true"
```
