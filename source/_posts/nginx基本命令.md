---
title: nginx 基本命令
date: 2022-03-11 11:59:33.747
updated: 2022-03-11 12:03:36.998
url: /archives/nginxji-ben-ming-ling
description: 简介介绍nginx基本命令
categories: 
- nginx
tags: 
- nginx
---

# nginx基本命令
1、启动：
使用默认nginx.conf来启动
```bash
start nginx
```
如果要指定配置文件来启动则使用以下命令即可：
```bash
nginx -c ./conf/jason.conf
```
2、关闭：

快速关闭nginx服务。
```bash
nginx -s stop
``` 
优雅的关闭,优雅是指当一个请求被处理完成之后才被关闭。
```bash
nginx -s quit
``` 

3、配置语法检查：可进行配置文件的语法检测。
```bash
nginx -c ./conf/jason.conf -t
``` 
4、查看nginx版本信息：-v和-V，一个小写v，一个大写V，两个的含义有些不同。

nginx -v:只是显示nginx的当前版本,如下图

![image.png](/upload/2022/03/image-649783c8200a4d018e2558a7976cc38d.png)

nginx -V：显示nginx版本、编译器版本和配置参数信息，如下图

![image.png](/upload/2022/03/image-61236735bba34682926b603790610f4c.png)

5、重新加载配置文件，nginx是支持热部署的，意思就是可以在不停止服务的情况下进行更新部署。
```bash
nginx -s reload 
```
6、linux命令重启
查找当前nginx进程号
```bash
ps -ef|grep nginx
```
然后输入命令：
```bash
kill -HUP 进程号
``` 
实现重启nginx服务

7、重新打开日志文件。
```bash
nginx -s reopen
``` 
