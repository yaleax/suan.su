---
title: Debian10 升级 Debian 11
date: 2021-09-02T04:11:49.773Z
lastmod: 2021-09-02T04:11:49.852Z
---
[Debian](https://www.debian.org/) 的大版本发布是很罕见的，因为它往往需要社区的多年努力。这就是为什么 Debian 是真正的通用操作系统，并且在稳定性方面坚如磐石。

代号 Bullseye 的 [Debian 11](https://www.debugpoint.com/2021/05/debian-11-features/) 即将正式发布。2021 年 7 月 15 日，Debian 11 进入完全冻结状态，这意味着发行在即。虽然官方发布日期还没有最终确定，但你现在就可以从 Debian 10 安装或升级到 Debian 11。

以下是方法。

### 前提条件

- 升级的过程非常简单明了。然而，采取某些预防措施是一个好的做法。特别是如果你正在升级一台服务器。
- 对你的系统进行备份，包括所有重要的数据和文件。
- 尝试禁用/删除你可能在一段时间内添加的任何外部仓库（PPA）。你可以在升级后逐一启用它们。
- 关闭所有正在运行的应用。
- 停止任何你可能已经启用的运行中的服务。升级完成后，你可以通过 [systemctl](https://www.debugpoint.com/2020/12/systemd-systemctl-service/) 启动它们。这包括 Web 服务器、SSH 服务器、FTP 服务器或任何其他服务器。
- 确保你有稳定的互联网连接。
- 并为你的系统留出足够的停机时间。因为根据你的系统配置，Debian 版本升级需要时间大约在 1.5 小时到 2 小时之间。

### 将 Debian 10 Buster 升级到 11 Bullseye

确保你的系统是最新的，而且你的软件包列表是最新的。

```
sudo apt update && sudo apt upgrade
```

使用下面的命令安装 `gcc-8-base` 包。这是必须的，因为在历史上曾出现过升级失败的情况，这是因为下面的软件包中包含了某些依赖。

```
sudo apt install gcc-8-base
```

![upgrade debian – system check](https://img.linux.net.cn/data/attachment/album/202108/04/114435o024zj0x0hy4vtxm.jpg)

打开 ` /etc/apt/sources.list`，通过注释 Debian 10 buster 包，而使用 bullseye 仓库进行更新。

、、、
nano /etc/apt/sources.list
、、、

注释所有的 buster 仓库，在行的开头加上 `#`。

![Comment the Debian 10 lines](https://img.linux.net.cn/data/attachment/album/202108/04/114436hmapipjumm4k5443.jpg)

在文件的末尾添加以下几行。

```
deb http://deb.debian.org/debian bullseye main contrib non-free
deb http://deb.debian.org/debian bullseye-updates main contrib non-free
deb http://security.debian.org/debian-security bullseye-security main
deb http://ftp.debian.org/debian bullseye-backports main contrib non-free
```

![Add Debian 11 lines](https://img.linux.net.cn/data/attachment/album/202108/04/114436n0qqjzqs43zqjv3q.jpg)

按 `Ctrl + O` 保存文件，按 `Ctrl + X` 退出 `nano`。

更新一次系统仓库列表，以验证仓库的添加情况。

```
sudo apt update
```

如果上面的命令没有出现任何错误，那么你已经成功地添加了 bullseye 仓库。

现在，通过运行下面的命令开始升级过程。基本安装的下载大小约为 1.2GB。这可能会根据你的系统配置而有所不同。

```
sudo apt full-upgrade
```

![Debian upgrade start](https://img.linux.net.cn/data/attachment/album/202108/04/114436z9i7iyq7xkzy9iec.jpg)


这个命令需要时间。但不要让系统无人看管。因为升级过程中需要各种输入。

![lib6 config](https://img.linux.net.cn/data/attachment/album/202108/04/114437isatsv93a9krva0r.jpg)


![sudoers file](https://img.linux.net.cn/data/attachment/album/202108/04/114437iknli2hdali27tpk.jpg)


完成后，你可以用以下命令重启系统。

```
systemctl reboot
```

重启后，运行以下命令，以确保你的系统是最新的，并且清理了所有不再需要的不必要的软件包。

```
sudo apt --purge autoremove
```

如果一切顺利，你应该看到了 Debian 11 bullseye。你可以用下面的命令来验证版本：

```
cat /etc/os-release
```

-------
抄袭：技术中国 (https://linux.cn/article-13647-1.html)