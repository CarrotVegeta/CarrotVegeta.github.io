---
abbrlink: ''
categories:
- - nginx
date: '2023-08-23T23:59:30.646913+08:00'
excerpt: 编译nginx 下载源码并解压 wget http://nginx.org/download/nginx-1.24.0.tar.gz tar -zxvf nginx-1.24.0.tar.gz  安装...
tags:
- nginx
title: 编译nginx源码并制作镜像
updated: 2023-8-24T0:12:52.816+8:0
---
## 编译nginx

### 下载源码并解压

```bash
wget http://nginx.org/download/nginx-1.24.0.tar.gz
tar -zxvf nginx-1.24.0.tar.gz
```

### 安装环境并编译

```bash
apt-get update
apt-get install -y build-essential libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
cd nginx-1.24.0
./configure
make
make install
```

## 制作镜像

拷贝相关文件到Dockerfile目录

```bash
cp /usr/local/nginx ./
```

生成一个conf文件

```bash
server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

```

生成一个nginx.conf文件

```bash
#user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;
    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}

```

编写Dockerfile文件

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

制作镜像

```bash
docker build -t nginx:1.24.0 .
```
