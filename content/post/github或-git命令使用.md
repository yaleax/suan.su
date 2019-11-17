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
git init 创建版本
git add 库添加文件到缓冲区
git commit -m ‘xxx’ 提交到本地仓库

git status 查看当前目录状态
git diff 查看文件变化的内容
git reset –hard HEAD 回退到上一版本, git reset –hard HEAD^ 回退到上上版本,git reset –hard HEAD~100 回退到上100个版本，git reset —hard 回退到指定版本
git reflog 查看历史版本，如果有需要，随后可以根据显示的commit id进行回退
git add后如果又修改了，此时git commit只会提交add过的版本，后面修改的不会提交
git checkout – revert文件到add过或最新的版本，先add的版本，再最新的版本
git reset HEAD 可以撤销git add的版本

![](https://img.suan.su/github.png)



