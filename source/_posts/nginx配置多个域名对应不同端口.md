---
title: nginx 配置多个域名对应不同端口
date: 2022-03-11 11:03:57.304
updated: 2022-03-11 11:25:17.058
url: /archives/nginxpei-zhi-duo-ge-yu-ming-dui-ying-bu-tong-duan-kou
categories: 
- nginx
tags: 
- nginx
---

# nginx 多个域名不同端口
## 首先准备三个域名和端口
- carrotvegeta.icu  80
- blog.carrotvegeta.icu 8090
- email.carrotvegeta.icu 8080
<!--more-->
## 1、写在一个配置文件里面（nginx.conf）：

```bash
server{
    listen 80;
    server_name carrotvegeta.icu;
    location / {
        #....
        proxy_pass http://localhost:80;
    }
    ##### other directive
}

server{
    listen 80;
    server_name blog.carrotvegeta.icu;
    location / {
        #....
        proxy_pass http://localhost:8090;
    }
    ##### other directive
}
```

如果要再继续增加就再增加一个server

```bash
server{
    listen 80;
    server_name carrotvegeta.icu;
    location / {
        #....
        proxy_pass http://localhost:80;
    }
    ##### other directive
}

server{
    listen 80;
    server_name blog.carrotvegeta.icu;
    location / {
        #....
        proxy_pass http://localhost:8090;
    }
    ##### other directive
}
server{
    listen 80;
    server_name email.carrotvegeta.icu;
    location / {
        #....
        proxy_pass http://localhost:8080;
    }
    ##### other directive
}
```
## 2、写在多个配置文件里面
  当我们的域名变的非常多的时候，就需要一直不断的在一个配置文件里面增加server，这样就会变得越来越多导致不太好管理。
  nginx支持引入文件的方法，这时我们可以在其他地方新建好我们所需要的配置文件:

blog.conf：
```bash
server{
    listen 80;
    server_name blog.carrotvegeta.icu;
    location / {
        #....
        proxy_pass http://localhost:8090;
    }
    ##### other directive
}
```

email.conf

```bash
server{
    listen 80;
    server_name email.carrotvegeta.icu;
    location / {
        #....
        proxy_pass http://localhost:8080;
    }
    ##### other directive
}
```

把两个文件都放在/data/nginx/conf/vhost目录下。

然后在nginx.conf中使用引入命令：
```bash
include  /data/nginx/conf/vhost/*.conf;即可。
```
需要注意的是这句命令应该放在：http{}  的花括号内。因为include的命令引入相当于被引入的所有代码写在nginx.conf中一样。

配置nginx.conf文件：

```bash
http{

  ......

  include /data/nginx/conf/vhost/*.conf;

  server{
    listen 80;
    server_name carrotvegeta.icu;
    location / {
        #....
        proxy_pass http://localhost:80;
    }
    ##### other directive
  }
}
```

然后重启ngxin:
```bash
nginx -s reload
```

原文地址：https://www.cnblogs.com/goloving/p/9363490.html
