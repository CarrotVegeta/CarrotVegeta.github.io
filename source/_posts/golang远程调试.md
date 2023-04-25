---
abbrlink: ''
categories:
- golang
date: '2023-04-11 23:44:01'
excerpt: golang远程调试 第一步：安装go ```shell 下载go wget https://dl.google.com/go/go1.20.3.linux-amd64.tar.gz
  解压 tar -xzf go1.20.3.linux-amd64.tar.gz 移动解压的目录到/usr/local/src目录下 mv go /usr/local/src
  配置环境变量 export PATH=$P...
tags:
- golang
title: golang远程调试
updated: Tue, 25 Apr 2023 16:05:08 GMT
url: /archives/golang-remote-debug
---
# golang远程调试

## 第一步：安装go

```shell
#下载go
wget https://dl.google.com/go/go1.20.3.linux-amd64.tar.gz
#解压
tar -xzf go1.20.3.linux-amd64.tar.gz
#移动解压的目录到/usr/local/src目录下
mv go /usr/local/src
#配置环境变量
export PATH=$PATH:/usr/local/src/go/bin
#使profile配置立即生效
source /etc/profile
#查看go版本
go version
#若显示 go version go1.20.3 linux/amd64 则成功
# 查看环境变量
go env 
#设置 goproxy
go env -w GOPROXY=https://goproxy.io,direct
```

## 第二步：安装dlv

```shell
go install github.com/go-delve/delve/cmd/dlv@latest
#移动dlv工具到bin目录下
mv dlv /usr/local/src/go/bin
```

## 第三步 启动dlv 实例

### 1、编译运行程序

```shell
#编译运行文件
go build -gcflags='all -N -l' main.go
```

- -N:禁止编译器优化
- -l:关闭内联结

### 2、dlv attach

这个相当于gdb -p 或者 gdb attach ，即跟踪一个正在运行的程序。这中用法也是很常见，对于一个后台程序，它已经运行很久了，此时你需要查看程序内部的一些状态，只能借助attach.

```shell
dlv attach --headless --listen ":2345" --log --api-version 2  4977 ## 后面的进程的ID
```

### 3、dlv直接运行

```shell
 dlv --listen=:2345 --headless=true --api-version=2 --accept-multiclient exec ./main
```

## goland 远程调试

打开goland配置 选择go remote 填写服务器地址和端口

![http://img.carrotvegeta.icu/blog/2023-4-04a0eafad3b9f3430e02fd50cf4afb26.jpg](http://img.carrotvegeta.icu/blog/2023-4-04a0eafad3b9f3430e02fd50cf4afb26.jpg)
