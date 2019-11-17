---
title: 找回Macbook Pro电脑丢失管理员权限
date: 2019-11-17T11:23:41.194Z
lastmod: 2019-11-17T11:23:42.509Z
tags:
  - MacOS
---
## 前言
我修改了Mac电脑的用户名，然后就丢失了 administrator 权限，太奇怪了，找了好久，才解决问题，记录一下。
## 解决步骤
1. 重新启动电脑。

2. 同时按 <kbd>Command</kbd> + <kbd>S</kbd> 。

3. 一直按住，直到有命令行画面显示。

4. 输入命令`/sbin/fsck -fy`然后按 <kbd>enter</kbd> 。

5. 输入命令`/sbin/mount -uw /`然后按 <kbd>enter</kbd> 。

6. 输入命令`rm /var/db/.AppleSetupDone`然后按 <kbd>enter</kbd> 。

7. 最后，输入`reboot`然后按 <kbd>enter</kbd> 电脑会重启。

## :tada:完成
重新开机，你可以重新配置你的 mac 电脑了，祝你好运。


