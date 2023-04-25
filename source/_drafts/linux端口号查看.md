---
abbrlink: ''
categories: []
date: '2023-04-25 23:08:04'
tags: []
title: linux端口号查看
updated: Tue, 25 Apr 2023 15:12:46 GMT
---
# linux端口号查看


1. nmap用于查看服务器对外开放的端口

   安装

   ```bash
   yum -y install nmap
   ```

   查看本机对外端口

   ```bash
   nmap 127.0.0.1
   ```
2. netstat命令 用于查看本机端口号占用
   安装

   ```bash
   yum -y install net-tools
   ```

查看端口占用

```bash
netstat -anp|grep 3306
```
