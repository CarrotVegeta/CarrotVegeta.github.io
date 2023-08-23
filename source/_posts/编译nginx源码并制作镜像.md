---
abbrlink: ''
categories: []
date: '2023-08-23T23:59:30.646913+08:00'
excerpt: 编译nginx 下载源码在本地编译 wget http://nginx.org/download/nginx-1.25.2.tar.gz tar -zxvf nginx-1.24.0.tar.gz a...
tags: []
title: 编译nginx源码并制作镜像
updated: 2023-8-24T0:5:22.662+8:0
---
## 编译nginx

下载源码在本地编译

```bash
wget http://nginx.org/download/nginx-1.25.2.tar.gz
tar -zxvf nginx-1.24.0.tar.gz
apt-get update
apt-get install -y build-essential libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
cd nginx-1.24.0
./configure
make
make install
```

制作镜像

```bash
FROM ubuntu:latest

COPY ./nginx/ /usr/local/nginx/ 
COPY ./nginx/conf/* /etc/nginx/
COPY ./nginx.conf /etc/nginx/

WORKDIR /usr/local/nginx/html
RUN  mkdir /etc/nginx/conf.d && mkdir /var/log/nginx && touch /var/log/nginx/error.log
COPY ./default.conf /etc/nginx/conf.d
ENV PATH=$PATH:/usr/local/nginx/sbin
EXPOSE 80
ENTRYPOINT ["nginx","-g","daemon off;"]
CMD ["-c","/etc/nginx/nginx.conf"]

```
