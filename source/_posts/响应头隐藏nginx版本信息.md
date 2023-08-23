---
abbrlink: ''
categories: []
date: '2023-08-24T00:13:12.339934+08:00'
excerpt: 隐藏nginx版本号 打开nginx.conf 在http块中增加 #server_tokens off;  重启 sudo systemctl reload nginx  隐藏nginx名称 修改源...
tags: []
title: 响应头隐藏nginx版本信息
updated: 2023-8-24T0:20:50.248+8:0
---
## 隐藏nginx版本号

打开nginx.conf

在http块中增加

```bash
#server_tokens off;
```

重启

```bash
sudo systemctl reload nginx
```

## 隐藏nginx名称

修改源码并重新编译

```bash
wget http://nginx.org/download/nginx-1.24.0.tar.gz 
tar -zxvf nginx-1.24.0.tar.gz
cd nginx-1.24.0/src/http
vim ngx_http_header_filter_module.c 
```

修改文件

```bash
#修改Server为其他名称或留空
static u_char ngx_http_server_string[] = "Server: nginx" CRLF;
static u_char ngx_http_server_full_string[] = "Server: " NGINX_VER CRLF;
static u_char ngx_http_server_build_string[] = "Server: " NGINX_VER_BUILD CRLF;
```

编译

```bash
apt-get update
apt-get install -y build-essential libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
cd nginx-1.24.0
./configure
make
make install
```
