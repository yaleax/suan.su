---
title: VPS连接 Google Drive
date: 2019-11-30T11:22:01.529Z
lastmod: 2019-11-30T11:22:02.630Z
---

有人已经写的挺好了，我为啥还要写？

[去教程](https://sunpma.com/567.html)!

## 一、前言

如果那个教程没有了，该怎么办？还是备份一下吧，照抄！重新排版！

## 二、必要条件
这个应该是 Ubuntu/Debian 系统比较实用，别的系统命令有细微差别。

## 三、rclone
### 1.安装 rclone
```bash
curl https://rclone.org/install.sh | sudo bash
```
### 2.开始配置
```bash
rclone config
```
### 3.详细配置
```bash
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n   #输入n回车，新建配置
name> GD   #新建配置的名称，随意填写，后面会用到
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / 1Fichier
   \ "fichier"
 2 / Alias for an existing remote
   \ "alias"
 3 / Amazon Drive
   \ "amazon cloud drive"
 4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, etc)
   \ "s3"
 5 / Backblaze B2
   \ "b2"
 6 / Box
   \ "box"
 7 / Cache a remote
   \ "cache"
 8 / Dropbox
   \ "dropbox"
 9 / Encrypt/Decrypt a remote
   \ "crypt"
10 / FTP Connection
   \ "ftp"
11 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
12 / Google Drive
   \ "drive"
13 / Google Photos
   \ "google photos"
14 / Hubic
   \ "hubic"
15 / JottaCloud
   \ "jottacloud"
16 / Koofr
   \ "koofr"
17 / Local Disk
   \ "local"
18 / Mega
   \ "mega"
19 / Microsoft Azure Blob Storage
   \ "azureblob"
20 / Microsoft OneDrive
   \ "onedrive"
21 / OpenDrive
   \ "opendrive"
22 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
23 / Pcloud
   \ "pcloud"
24 / Put.io
   \ "putio"
25 / QingCloud Object Storage
   \ "qingstor"
26 / SSH/SFTP Connection
   \ "sftp"
27 / Union merges the contents of several remotes
   \ "union"
28 / Webdav
   \ "webdav"
29 / Yandex Disk
   \ "yandex"
30 / http Connection
   \ "http"
31 / premiumize.me
   \ "premiumizeme"
Storage> 12       #选择需要挂载的网盘
** See help for drive backend at: https://rclone.org/drive/ **

Google Application Client Id
Setting your own is recommended.
See https://rclone.org/drive/#making-your-own-client-id for how to create your own.
If you leave this blank, it will use an internal key which is low performance.
Enter a string value. Press Enter for the default ("").
client_id>        #默认直接回车，或者输入自己的OAuth ID
Google Application Client Secret
Setting your own is recommended.
Enter a string value. Press Enter for the default ("").
client_secret>    #默认直接回车，或者输入自己的OAuth秘锁
Scope that rclone should use when requesting access from drive.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / Full access all files, excluding Application Data Folder.
   \ "drive"
 2 / Read-only access to file metadata and file contents.
   \ "drive.readonly"
   / Access to files created by rclone only.
 3 | These are visible in the drive website.
   | File authorization is revoked when the user deauthorizes the app.
   \ "drive.file"
   / Allows read and write access to the Application Data folder.
 4 | This is not visible in the drive website.
   \ "drive.appfolder"
   / Allows read-only access to file metadata but
 5 | does not allow any access to read or download file content.
   \ "drive.metadata.readonly"
scope> 1          #默认输入1回车
ID of the root folder
Leave blank normally.
Fill in to access "Computers" folders. (see docs).
Enter a string value. Press Enter for the default ("").
root_folder_id>  #默认直接回车
Service Account Credentials JSON file path 
Leave blank normally.
Needed only if you want use SA instead of interactive login.
Enter a string value. Press Enter for the default ("").
service_account_file> 
Edit advanced config? (y/n)
y) Yes
n) No
y/n> n           #是否编辑高级配置，输入n回车
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes
n) No
y/n> n          #是否使用自动配置，输入n回车
If your browser doesn't open automatically go to the following link: https://accounts.google.com/o/oauth2/XXX
Log in and authorize rclone for access
Enter verification code> XXXX  #复制上面的链接在浏览器中打开，将得到的授权码复制到这里后回车
Configure this as a team drive?
y) Yes
n) No
y/n> y          #是否挂载团队网盘，如果没有输入n回车，需要挂载团队版输入y回车
Fetching team drive list...
Choose a number from below, or type in your own value
 1 / SunPma
   \ "0ADYACmKjlU3JUk9PVA"
Enter a Team Drive ID> 1      #选择你需要挂载的团队盘然后回车
--------------------
[SUNPMAGD]
type = drive
scope = drive
token = {"access_token":"XXX","expiry":"2019-10-23T14:32:06.481207275+08:00"}
team_drive = 0ADYACmKjlU3JUk9PVA
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y       #默认输入y回车
Current remotes:

Name                 Type
====                 ====
SUNPMAGD             drive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q   #输入q回车，保存配置退出
```
### 4.新建本地挂载文件夹
```bash
cd /root
mkdir emby
mkdir emby/config
mkdir rclone
mkdir rclone/ldfdsa00
```
{{% notice info 说明 %}}   
#/usr/bin/rclone mount DriveName:Folder LocalFolder \
下面整体代码中最上面的这条需要自己修改，不要直接复制，注意代码中有空格        
DriveName    #配置时填写的name    
Folder       #网盘里要挂载的文件夹名    
LocalFolder  #本地要挂载的文件夹绝对路径    
例：/usr/bin/rclone mount GD:VPS /home/GoogleDrive \
{{% /notice %}}

### 5.挂载
```bash
rclone mount ldfdsa00: /root/rclone/ldfdsa00  --buffer-size 1G --vfs-read-chunk-size 256M --vfs-read-chunk-size-limit 2G  --allow-non-empty --allow-other   --dir-cache-time 12h  >/dev/null 2>&1 &
```
### 6.开机自动启动
```bash
nano /etc/crontab
@reboot root  rclone mount ldfdsa00: /root/rclone/ldfdsa00  --buffer-size 1G --vfs-read-chunk-size 256M --vfs-read-chunk-size-limit 2G  --allow-non-empty --allow-other   --dir-cache-time 12h  >/dev/null 2>&1 &
```
### 6.1重新挂在
如果有问题，可以选择重新挂在
```bash
umount /root/rclone/ldfdsa00 #卸载源目录
rclone mount gd（换成新账号，内部目录要匹配）: /root/rclone/ldfdsa00  --buffer-size 1G --vfs-read-chunk-size 256M --vfs-read-chunk-size-limit 2G  --allow-non-empty --allow-other   --dir-cache-time 12h  >/dev/null 2>&1 &      #重新挂载
```
----
[参考1]<https://wzfou.com/rclone-cos-fuse-ossfs/>   
[参考2]<https://sunpma.com/567.html>   
[参考3]<http://dxz.plus/index.php/2019/11/03/10.html>   
