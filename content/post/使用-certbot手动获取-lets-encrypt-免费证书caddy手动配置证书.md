---
title: 使用 Certbot手动获取 Let's Encrypt 免费证书Caddy手动配置证书
date: 2019-11-19T06:57:22.078Z
lastmod: 2019-11-19T06:57:22.196Z
tags:
  - Caddy
  - 证书
---
## 一、前言

之前写了一个教程是利用 Caddy 自动获得证书得，最近总是失败，所以就选择手动获取，然后再配置 Caddy。首先我们需要从证书授权机构(以下简称 CA ) 处获取一个证书，Let's Encrypt 就是一个 CA。
 [Let's Encrypt](https://letsencrypt.org) 是一个免费、开放，自动化的证书颁发机构，Caddy自动申请得也是这家。
 [Certbot](https://certbot.eff.org) 是 Let’s Encrypt 官方推荐的证书生成客户端工具



## 二、教程

### 1.安装 Cerbot

```bash
# 下载 Certbot 客户端
wget https://dl.eff.org/certbot-auto

# 设为可执行权限
chmod a+x certbot-auto
```



### 2.申请通配符证书

Let’s Encrypt目前支持三种验证方式：

- dns-01：给域名添加一个 DNS TXT 记录。
- http-01：在域名对应的 Web 服务器下放置一个 HTTP well-known URL 资源文件。
- tls-sni-01：在域名对应的 Web 服务器下放置一个 HTTPS well-known URL 资源文件。

使用 Certbot 客户端申请证书方法非常的简单，只需如下一行命令就搞定了。

下面以 dns验证方式为例

```bash
./certbot-auto certonly  -d "*.xxx.com" --manual --preferred-challenges dns-01  --server https://acme-v02.api.letsencrypt.org/directory
```
{{% notice tip 提示%}}
1.申请通配符证书，只能使用 dns-01 的方式。  
2.`xxx.com` 请根据自己的域名自行更改。
{{% /notice %}}    

相关参数说明：

{{% notice info 说明%}}
certonly 表示插件，Certbot 有很多插件。不同的插件都可以申请证书，用户可以根据需要自行选择。
-d 为哪些主机申请证书。如果是通配符，输入 *.xxx.com (根据实际情况替换为你自己的域名)。
--preferred-challenges dns-01，使用 DNS 方式校验域名所有权。
--server，Let's Encrypt ACME v2 版本使用的服务器不同于 v1 版本，需要显示指定。
{{% /notice %}}    


![](https://img.suan.su/le_ssl_01.png)

第五步 更改 dnstxt

测试验证

```
dig  -t txt _acme-challenge.xxx.com @8.8.8.8 
```

确认生效后，回车继续执行，最后会输出如下内容：

```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/xxx.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/xxx.com/privkey.pem
   Your cert will expire on 2018-06-12. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot-auto
   again. To non-interactively renew *all* of your certificates, run
   "certbot-auto renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

到了这一步后，恭喜您，证书申请成功。 证书和密钥保存在下列目录：

```
$ tree /etc/letsencrypt/live/xxx.com/
.
├── cert.pem
├── chain.pem
├── fullchain.pem
└── privkey.pem
```

校验证书信息，输入如下命令：

```
$ openssl x509 -in  /etc/letsencrypt/live/xxx.com/cert.pem -noout -text 

# 可以看到证书包含了 SAN 扩展，该扩展的值就是 *.xxx.com
...
Authority Information Access: 
        OCSP - URI:http://ocsp.int-x3.letsencrypt.org
        CA Issuers - URI:http://cert.int-x3.letsencrypt.org/

X509v3 Subject Alternative Name: 
    DNS:*.xxx.com
...
```

到此，我们就演示了如何在 Let’s Encrypt 申请免费的通配符证书。
