---
title: Github或 Git命令使用
date: 2019-11-17T11:02:02.027Z
lastmod: 2019-11-17T11:02:04.017Z
tags:
  - Github
---
## 前言
使用了Hugo静态博客后，才开始正式使用GitHub，经过一段时间的使用，发展GitHub真好用，现在把一些关键信息记录下来。

## GitHub

### 1.生成密钥
生成密钥后，需要把密钥复制到GitHub的 SSH keys里

```bash
ssh-keygen -t rsa -C "`此处填写你的邮箱地址`"
```
### 2.初始化
进入想建立 Git 的文件夹
```bash
git init
git add .
git commit -m "first commit"
git remote add origin git@github.com:替换成你的 GitHub用户名/3cho.git
git push -u origin master
```
### 3. 下载更新

```bash
git push
```

### GitHub命令速查表

![](https://img.suan.su/github.png)



