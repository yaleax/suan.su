---
title: 使用Hugo +GitHub + Netlify+Netlify CMS，来制作个人免费静态博客
description: 使用Hugo +GitHub + Netlify+Netlify CMS，来制作个人免费静态博客
date: 2019-11-09T06:46:37.869Z
lastmod: 2019-11-09T06:46:37.895Z
tags:
  - hugo
  - 博客
weight: 0
---
## 前言

这是一篇手动创建Hugo博客的教程，跟着教程一步一步的进行，最终你将会得到一个类似我这样的博客，在这个过程中，如果你能用心思考，善用搜索，你将会理解，这些服务彼此是如何连接的，每一项服务的功能是什么，以及如何修改它们的配置文件。理解这些后，你就可以根据自己的需求，创建自己的博客组合了。如果不想手动创建，你也可以使用 [Stackbit](https://www.stackbit.com/)来自动创建博客。它支持Hugo,Gatsby和Jekyll，后台管理支持Netlify CMS，Forestry和Contentful。关于[Stackbit的教程](https://suan.su/post/%E4%BD%BF%E7%94%A8stackbit-%E6%9D%A5%E5%BF%AB%E9%80%9F%E6%90%AD%E5%BB%BA%E9%9D%99%E6%80%81%E5%8D%9A%E5%AE%A2/)，以后会写，先挖坑。

## 一、技术总结

经过一周的摸索和实践，终于把这个博客搭建成功了。简单总结一下本博客所使用的技术。

* 博客生成平台：[Hugo](https://gohugo.io/)
* 博客主题：[Jane](https://github.com/xianmin/hugo-theme-jane)
* 博客代码托管：[GitHub](https://github.com/)
* 博客服务器供应商：[Netlify](https://www.netlify.com/)
* 博客后台管理平台：[Netlify CMS](https://www.netlifycms.org/)
* 域名服务商：[RuTLD](https://ru-tld.ru/en/)
* DNS服务商：[CloudFlare](https://www.cloudflare.com/)
* 域名证书：[CloudFlare](https://www.cloudflare.com/)
* CDN网络加速：[CloudFlare](https://www.cloudflare.com/)
* 图床：[BackBlaze](https://www.backblaze.com/)

上面提到的这些服务，除了购买域名需要花钱，其他服务都是免费的，购买域名也不是必要条件， Netlify 可以提供的二级域名。 

## 二、必要条件

1.注册 [GitHub](https://github.com/join)
2.注册 [Netlify](https://app.netlify.com/signup)
3. 注册 [CloudFlare](https://dash.cloudflare.com/sign-up)
4. 一台可以访问网络的计算机

## 三、博客使用的服务简介

1. [Hugo](https://gohugo.io/) 是使用 Go 语言编写的，静态博客生成平台。使用Hugo平台，你可以快速的生成博客页面，因为是静态博客，所以访问者的载入速度是非常快的，静态博客对爬虫是很友好的，这样就利于被搜索引擎抓取网站。
2. [Netlify](https://www.netlify.com/) 提供免费的静态博客托管服务和免费二级域名：<https://suan.netlify.com>。
3. [GitHub](https://github.com/)提供免费代码托管服务，Hugo 生成的代码，会存在这里里。
4. [Netlify CMS](https://www.netlifycms.org/)是一套开源的后台管理平台，支持 Markdown 语法，给习惯在后台编辑文章的人使用。
5. [CloudFlare](https://www.cloudflare.com/)拥有全球最大的网络之一，提供 CDN,DNS,ssl证书等服务,远不止这些。

## 四、安装 Hugo

### 1.Ubuntu系统：

先去 [Hugo release](https://github.com/gohugoio/hugo/releases)查看 Hugo的最新版本，用最新版本号替代下面代码里面的0.59.1，你也可以直接复制下面的代码，进行下载和安装。

```bash
#使用 wget 下载 hugo
wget https://github.com/gohugoio/hugo/releases/download/v0.59.1/hugo_0.59.1_Linux-64bit.deb
#使用 dpkg 安装 .deb 文件
sudo dpkg -i hugo_0.59.1_Linux-64bit.deb
```

### 2.MacOS 系统：

```bash
brew install hugo
```

如果你没有[brew](https://brew.sh/)包工具，你可以用下面的命令安装brew包工具

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 五、使用 Hugo创建静态博客

### 1.创建网站存放目录

```bash
# 使用 mkdir命令，创建网站目录
mkdir -p /var/www
# 使用 cd命令，进入创建的目录
cd /var/www
# 新建站点，名字以3cho为例,你可以替换成自己的名字
hugo new site 3cho
```

### 2.安装主题，主题可以去Hugo[官方主题库](https://themes.gohugo.io/)下载,本文以 [even](https://github.com/olOwOlo/hugo-theme-even) 主题为例

```bash
# 进入新建好的3cho目录
cd /var/www/3cho
# 安装主题（不同主题安装方式不同）
git clone https://github.com/olOwOlo/hugo-theme-even themes/even
#复制主题自带的 config.toml文件到当前根目录
cp themes/even/exampleSite/config.toml ./config.toml
```

### 3.创建一篇测试新文章

```bash
#生成一篇新文章
hugo new post/hello.md
```

### 4.生成博客静态博客

```bash
# 生成静态网页，包括草稿，生成好的内容在public目录中
hugo -D
```

### 5.启动本地博客服务器

```bash
hugo server
```

点击访问你的博客<http://localhost:1313>

> 到这里，博客搭建工作已经完成了，接下来是把这个博客，部署到网络上，这样其他人才可以访问你的博客了。

### 6.关闭本地博客服务器

使用快捷键 <kbd>ctrl</kbd>+<kbd>c</kbd> 关闭本地博客服务器

## 六、GitHub使用

### 1.创新一个新的Github仓库

Create a new repository  repository在 GitHub[点击查看官方教程](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository)，你不需要看文字，跟着图走就行。repository名字是什么很重要，下面我们要用它来替换3cho。

### 2.初始化 GitHub，初始化意思是初次建立本地目录和 GitHub远程仓库的连接

```bash
#仓库名字repository以3cho为例
echo "# 3cho" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:替换成你的 GitHub用户名/3cho.git
git push -u origin master
```

## 七、连接Netlify前的准备

### 1.删除主题下面的.git文件，主题以 even为例

```Bash
cd /var/www/3cho/themes/even
rm -rf .git
```

### 2.创建 Netlify 需要的配置文件

```Bash
nano netlify.toml
```

### 3.复制下面的文件粘贴进去，Hugo version后面的数字替换成，你安装的版本。不明白什么意思，可以不改。

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

粘贴完成后，你需要同时按 <kbd>ctrl</kbd>+<kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按<kbd>回车</kbd>确认保存。

### 4.创建 Netlify CMS所需要的配置文件和目录

```bash
touch static/.keep data/.keep
```

### 5.创建Netlify CMS需要用的 admin 和 img文件夹

```bash
mkdir /var/www/3cho/static/admin
mkdir /var/www/3cho/static/img
```

### 6.创建Netlify CMS页面

```bash
nano /var/www/3cho/static/admin/index.html
```

### 7.复制下面的文件粘贴进去

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Content Manager</title>
    <!-- Include the script that enables Netlify Identity on this page. -->
    <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
  </head>
  <body>
    <!-- Include the script that builds the page and powers Netlify CMS -->
    <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
  </body>
</html>
```

粘贴完成后，你需要同时按 <kbd>ctrl</kbd>+<kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按<kbd>回车</kbd>确认保存。

### 8.创建Netlify CMS配置文件

```bash
nano /var/www/3cho/static/admin/config.yml
```

### 9.复制下面的文件粘贴进去

```yml
backend:
  name: git-gateway
  branch: master
media_folder: static/img
public_folder: /img
collections:
  - name: config
    label: Config
    files:
      - name: config
        label: Config
        file: config.toml
        fields:
          - widget: string
            name: title
            label: Title
            required: false
          - widget: string
            name: baseURL
            label: Base URL
            required: false
            hint: Hostname (and path)  to the root
          - widget: boolean
            name: enableRobotsTXT
            label: Enable Robots Text
            required: false
          - widget: boolean
            name: enableEmoji
            label: Enable Emoji
            required: false
          - widget: string
            name: theme
            label: Theme name
            required: false
          - widget: boolean
            name: hasCJKLanguage
            label: Has CJK Language
            required: false
          - widget: number
            name: paginate
            label: Paginate
            required: false
            hint: Number of articles displayed on the homepage
            valueType: int
          - widget: number
            name: rssLimit
            label: Rss Limint
            required: false
            hint: Limit Entry Count to Rss file
            valueType: int
          - widget: string
            name: disqusShortname
            label: Disqus Shortname
            required: false
          - widget: string
            name: googleAnalytics
            label: Google Analytics code
            required: false
          - widget: string
            name: copyright
            label: Copyright
            required: false
          - widget: string
            name: defaultContentLanguage
            label: Default Content Language
            required: false
          - widget: object
            name: languages
            label: Languages
            required: false
            fields:
              - widget: object
                name: en
                label: English
                required: false
                fields:
                  - widget: string
                    name: languageCode
                    label: Language code
                    required: false
          - widget: object
            name: author
            label: Author
            required: false
            fields:
              - widget: string
                name: name
                label: Name
                required: false
          - widget: object
            name: sitemap
            label: Sitemap
            required: false
            fields:
              - widget: string
                name: changefreq
                label: Change frequency
                required: false
              - widget: number
                name: priority
                label: Priority
                required: false
                valueType: float
              - widget: string
                name: filename
                label: Filename
                required: false
          - widget: object
            name: menu
            label: Menu
            required: false
            fields:
              - widget: list
                name: main
                label: Main Menu
                required: false
                fields:
                  - widget: string
                    name: name
                    label: Menu Name
                    required: false
                  - widget: number
                    name: weight
                    label: Page order weight
                    required: false
                    valueType: int
                  - widget: string
                    name: identifier
                    label: Identifier
                    required: false
                  - widget: string
                    name: url
                    label: Menu Link
                    required: false
          - widget: object
            name: params
            label: Site Parameters
            required: false
            fields:
              - widget: string
                name: since
                label: Since
                required: false
                hint: Site Creation Time
              - widget: boolean
                name: homeFullContent
                label: Home Full Content
                required: false
                hint: >-
                  if false, show post summaries on home page. Othewise show full
                  content.
              - widget: boolean
                name: rssFullContent
                label: Rss Full Content
                required: false
                hint: 'if false, Rss feed instead of the summary'
              - widget: string
                name: logoTitle
                label: Logo Title
                required: false
              - widget: list
                name: keywords
                label: Keywords
                required: false
                field:
                  label: String
                  name: string
                  widget: string
              - widget: string
                name: description
                label: description
                required: false
              - widget: string
                name: dateFormatToUse
                label: Date Format
                required: false
              - widget: boolean
                name: toc
                label: TOC
                required: false
              - widget: boolean
                name: photoswipe
                label: Photo Swipe
                required: false
              - widget: string
                name: contentCopyright
                label: Content Copyright
                required: false
              - widget: list
                name: customCSS
                label: Custom Css
                required: false
                hint: >-
                  if ['custom.css'], load '/static/css/custom.css' fileif
                  ['custom.css'], load '/static/css/custom.css' file
                field:
                  label: String
                  name: string
                  widget: string
              - widget: list
                name: customJS
                label: Custom JS
                required: false
                hint: 'if [''custom.js''], load ''/static/js/custom.js'' file'
                field:
                  label: String
                  name: string
                  widget: string
              - widget: object
                name: social
                label: Social
                required: false
                fields:
                  - widget: string
                    name: a-email
                    label: Email
                    required: false
                  - widget: string
                    name: b-stack-overflow
                    label: StackOverflow
                    required: false
                  - widget: string
                    name: c-twitter
                    label: Twitter
                    required: false
                  - widget: string
                    name: d-facebook
                    label: Facebook
                    required: false
                  - widget: string
                    name: e-linkedin
                    label: Linkedin
                    required: false
                  - widget: string
                    name: f-google
                    label: Google
                    required: false
                  - widget: string
                    name: g-github
                    label: Github
                    required: false
                  - widget: string
                    name: h-weibo
                    label: Weibo
                    required: false
                  - widget: string
                    name: i-zhihu
                    label: Zhihu
                    required: false
                  - widget: string
                    name: j-douban
                    label: Douban
                    required: false
                  - widget: string
                    name: k-pocket
                    label: Pocket
                    required: false
                  - widget: string
                    name: l-tumblr
                    label: Tumblr
                    required: false
                  - widget: string
                    name: m-instagram
                    label: Instagram
                    required: false
                  - widget: string
                    name: n-gitlab
                    label: Gitlab
                    required: false
                  - widget: string
                    name: o-goodreads
                    label: Goodreads
                    required: false
                  - widget: string
                    name: p-coding
                    label: Coding
                    required: false
                  - widget: string
                    name: q-bilibili
                    label: Bilibili
                    required: false
                  - widget: string
                    name: r-codeforces
                    label: Codeforces
                    required: false
                  - widget: string
                    name: s-mastodon
                    label: Mastodon
                    required: false
  - name: basicpage
    label: Basic pages
    folder: content/
    create: true
    extension: md
    slug: '{{slug}}'
    fields:
      - widget: string
        name: title
        label: Title
        required: false
      - widget: datetime
        name: date
        label: Publish Date
        required: false
      - widget: datetime
        name: lastmod
        label: Last Modified Date
        required: false
      - widget: string
        name: menu
        label: Menu
        required: false
      - widget: number
        name: weight
        label: Page Order weight
        required: false
        valueType: int
      - widget: boolean
        name: comment
        label: Comment
        required: false
      - widget: boolean
        name: mathjax
        label: Mathjax
        required: false
        hint: 'see https://www.mathjax.org/'
      - widget: markdown
        name: body
        label: Content
        required: false
        hint: Page content
  - name: post
    label: Blog postss
    folder: content/post
    create: true
    extension: md
    slug: '{{slug}}'
    fields:
      - widget: string
        name: title
        label: Blog Title
        required: false
      - widget: string
        name: description
        label: Description
        required: false
      - widget: string
        name: author
        label: Blog Author
        required: false
      - widget: datetime
        name: date
        label: Publish date
        required: false
      - widget: datetime
        name: lastmod
        label: Last Modified Date
        required: false
      - widget: list
        name: tags
        label: Tags
        required: false
        field:
          label: String
          name: string
          widget: string
      - widget: list
        name: categories
        label: Categories
        required: false
        field:
          label: String
          name: string
          widget: string
      - widget: boolean
        name: draft
        label: Draft
        required: false
      - widget: boolean
        name: comment
        label: Comment
        required: false
      - widget: boolean
        name: toc
        label: Toc
        required: false
      - widget: boolean
        name: autoCollapseToc
        label: Auto Collaps Toc
        required: false
      - widget: string
        name: contentCopyright
        label: Content Copyright
        required: false
      - widget: boolean
        name: reward
        label: Reward
        required: false
      - widget: boolean
        name: mathjax
        label: Mathjax
        required: false
        hint: 'see https://www.mathjax.org/'
      - widget: boolean
        name: katex
        label: Katex
        required: false
        hint: 'See https://github.com/KaTeX/KaTeX'
      - widget: string
        name: markup
        label: Markup
        required: false
        hint: 'See https://gohugo.io/content-management/formats/#mmark'
      - widget: number
        name: weight
        label: Weight
        required: false
        valueType: int
      - widget: object
        name: menu
        label: Menu
        required: false
        fields:
          - widget: object
            name: main
            label: Main
            required: false
            fields:
              - widget: string
                name: parent
                label: Parent Menu
                required: false
              - widget: number
                name: weight
                label: Weight
                required: false
                valueType: int
      - widget: markdown
        name: body
        label: Content
        required: false
        hint: Page content
```

粘贴完成后，你需要同时按 <kbd>ctrl</kbd>+<kbd>x</kbd>来退出,再输入<kbd>y</kbd>确认保存，再按<kbd>回车</kbd>确认保存。

### 10.上传到GitHub

```bash
#仓库名字repository以3cho为例
echo "# 3cho" >> README.md
git init
git add .
git commit -m "install"
git remote add origin git@github.com:替换成你的 GitHub用户名/3cho.git
git push -u origin master
```

## 八、Netlify连接 GitHub仓库

### 1.使用Github帐号登录[Netlify](https://app.netlify.com/)
![](https://img.suan.su/Screen-Shot-2019-11-11-16-30-16.18.png)
### 2.创建新的站点
![Netlfiy 创建新站点](https://img.suan.su/Screen-Shot-2019-11-11-16-07-20.png)
### 3.连接 Github
![](https://img.suan.su/Screen-Shot-2019-11-11-16-11-10.png)
### 4.选择博客说在的GitHub仓库
![](https://img.suan.su/Screen-Shot-2019-11-11-16-25-01.png)
### 5.部署
![](https://img.suan.su/Screen-Shot-2019-11-11-16-26-39.png)

> 等待完成，你的博客就已经成功创建了。Netlify 会提供一个二级域名。但是名字比较难记，你可以更改一个好记一些的。

### 6.更改二级域名名字
![](https://img.suan.su/Screen-Shot-2019-11-11-16-35-38.png)
![](https://img.suan.su/Screen-Shot-2019-11-11-16-36-01.png)
![](https://img.suan.su/Screen-Shot-2019-11-11-16-38-37.png)

> 大功告成

## 九、Netlify CMS

### 1.开启身份验证功能
![](https://img.suan.su/pb-W8ymYipYXE.png)
### 2.开启 Git Gateway
![](https://img.suan.su/pb-SXUAftKp1b.png)
### 3.开启邮箱免验证功能
![](https://img.suan.su/pb-XAnpBjmygb.png)
### 4.邀请自己成为管理员
![](https://img.suan.su/pb-BPUoTrJr83.png)
### 5.填写自己的邮箱  
### 6.去邮箱接收邀请邮箱    
### 7.Netlify CMS的后台地址是，你的网站后面加 admin，以 <https://suan.su> 为例，后台管理登录地址应该是:<https://suan.su/admin>  

## 后话

当我逐渐熟悉了 Markdown本地文本编辑后，我已经不太建议你使用 netlify cms了。
