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
## 写在最前面

这是一篇手动创建博客的教程，你也可以使用 Stackbit来自动创建博客。并且创建时选择性更多。那么你为什么还要手动创建呢？手动创建博客除了成就感更高一些，还能够帮助你理解，这些服务是如何组合在一起的，每一部分的功能是什么，以及如何修改他们的配置文件等等。如果你都理解了，那就试试自动创建吧。

## 零、总结

经过一周的摸索和实践，终于把这个博客搭建成功了。简单总结一下本博客所使用的技术服务有哪些。

* 博客生成平台：[Hugo](https://gohugo.io/)
* 博客主题：[Jane](https://github.com/xianmin/hugo-theme-jane)
* 博客代码托管：[GitHub](https://github.com/)
* 网站服务器供应商：[Netlify](https://www.netlify.com/)
* 博客后台管理平台：[Netlify CMS](https://www.netlifycms.org/)
* 域名服务商：[RuTLD](https://ru-tld.ru/en/)
* DNS服务商：[CloudFlare](https://www.cloudflare.com/)
* 域名证书：[CloudFlare](https://www.cloudflare.com/)
* CDN网络加速：[CloudFlare](https://www.cloudflare.com/)
  
上面提到的这些服务，除了购买域名需要花钱，其他服务都是免费的，购买域名也不是必要条件， Netlify 可以提供的二级域名。 

## 一、前置条件

1. 点击注册 [GitHub](https://github.com/join)
2. 点击注册 [Netlify](https://app.netlify.com/signup)
3. 点击注册 [CloudFlare](https://dash.cloudflare.com/sign-up)
4. 一台可以访问网络的计算机（废话）

## 二、Hugo 介绍

1. Hugo 是使用 Go 语言编写的，静态博客生成平台。使用Hugo平台，你可以快速的生成博客页面，因为是静态博客，所以访问者的载入速度是非常快的，静态博客对爬虫是很友好的，这样就利于被搜索引擎抓取网站。
2. 多平台支持，你可以在自己的电脑上搭建Hugo平台，然后把生成好的静态博客推送到 GitHub里。放心，主流的操作系统都支持安装 hugo。
3. Netlify 提供免费的静态博客托管服务和免费二级域名,<https://suan.netlify.com>。
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

2.安装主题，主题可以去Hugo[官方主题库](https://themes.gohugo.io/)下载,本文以\[even](https://github.com/olOwOlo/hugo-theme-even themes/even)主题为例

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
```

4.生成博客静态博客

```bash
# 生成静态网页，包括草稿，生成好的内容在public目录中
hugo -D
```

5.启动本地博客服务器

```bash
hugo server
```

点击访问你的博客<http://localhost:1313>

到这里，博客搭建工作已经完成了，接下来是把这个博客，部署到网络上，这样其他人才可以访问你的博客了。

6.关闭本地博客服务器

> 键盘同时按Control建和c建，关闭本地博客服务器

## 五、GitHub

1.创新一个新的Github仓库

Create a new repository  repository在 GitHub[点击查看官方教程](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository)，你不需要看文字，跟着图走就行。repository名字是什么很重要，下面我们要用它来替换3cho。

2.初始化 GitHub，初始化意思是初次建立本地目录和 GitHub远程仓库的连接

```bash
#仓库名字repository以3cho为例
echo "# 3cho" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:替换成你的 GitHub用户名/3cho.git
git push -u origin master
```

## 六、连接Netlify前的准备

1.删除主题下面的.git文件，主题以 even为例

```Bash
cd /var/www/3cho/themes/even
rm -rf .git
```

2.创建 Netlify 需要的配置文件

```Bash
nano netlify.toml
```

3.复制下面的文件粘贴进去，Hugo version后面的数字替换成，你安装的版本。不明白什么意思，可以不改。

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

粘贴完成后，你需要同时按control x 退出,输入 y 确认保存，再按回车确认保存当前名字。
4.创建 Netlify CMS所需要的配置文件和目录

```bash
touch static/.keep data/.keep
```

5.创建Netlify CMS需要用的 admin 和 img文件夹

```bash
mkdir /var/www/3cho/static/admin
mkdir /var/www/3cho/static/img
```

6.创建Netlify CMS页面

```bash
nano /var/www/3cho/static/admin/index.html
```

7.复制下面的文件粘贴进去

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

> 粘贴完成后，你需要同时按control x 退出,输入 y 确认保存，再按回车确认保存当前名字。

8.创建Netlify CMS配置文件

```bash
nano /var/www/3cho/static/admin/config.yml
```

9.复制下面的文件粘贴进去

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

> 粘贴完成后，你需要同时按control x 退出,输入 y 确认保存，再按回车确认保存当前名字。
