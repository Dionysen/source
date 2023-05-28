---
title: SSL 证书安装 on nginx
date: 2023-05-25 17:34:01
categories: Wiki
tags: [SSL, nginx]
comment: true
---

给服务器安装SLL证书以支持HTTPS

<!--more-->

请在 SSL 证书管理控制台中下载您需要安装的证书
`cloud. tencent. com_bundle. crt` 证书文件
`cloud. tencent. com_bundle. pem` 证书文件（可忽略该文件）
`cloud. tencent. com. key` 私钥文件
`cloud. tencent. com. csr CSR` 文件
查看 nginx 是否安装：

```bash
nginx -v
sudo apt install nginx
```

查看 nginx 的配置文件：

```bash
sudo nginx -t
# 显示为
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

编辑配置文件：

```bash
sudo vim /etc/nginx/nginx.conf
```

在 http 中添加：

```bash
server {
     #SSL 默认访问端口号为 443
     listen 443 ssl;
     #请填写绑定证书的域名
     server_name www.dionysen.top;
     #请填写证书文件的相对路径或绝对路径
     ssl_certificate /home/dionysen/.config/www.dionysen.top_nginx/www.dionysen.top_bundle.crt;
     #请填写私钥文件的相对路径或绝对路径
     ssl_certificate_key /home/dionysen/.config/www.dionysen.top_nginx/www.dionysen.top.key;
     ssl_session_timeout 5m;
     #请按照以下协议配置
     ssl_protocols TLSv1.2 TLSv1.3;
     #请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
     ssl_prefer_server_ciphers on;
     location / {
         #网站主页路径。此路径仅供参考，具体请您按照实际目录操作。
         #例如，您的网站主页在 Nginx 服务器的 /etc/www 目录下，则请修改 root 后面的 html 为 /etc/www。
         root html;
         index  index.html index.htm;
     }
 }
server {
 listen 80;
 #请填写绑定证书的域名
 server_name www.dionysen.top;
 #把http的域名请求转成https
 return 301 https://$host$request_uri;
}
```

测试配置文件有效性：

```bash
sudo nginx -t
```

重新载入：

```bash
sudo nginx -s reload
```
