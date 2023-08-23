---
abbrlink: ''
categories:
- - docker
date: '2023-08-23T12:38:26.373756+08:00'
excerpt: docker 设置镜像源 配置镜像源 vi /etc/docker/daemon.json  # 内容如下： {   &quot;registry-mirrors&quot;: [     &quot...
tags:
- docker
title: ''
updated: 2023-8-23T12:43:21.48+8:0
---
# docker 设置镜像源

## 配置镜像源

```bash
vi /etc/docker/daemon.json

# 内容如下：
{
  "registry-mirrors": [
    "https://xx4bwyg2.mirror.aliyuncs.com",
    "http://f1361db2.m.daocloud.io",
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ]
}{}

# 退出并保存
:wq

# 使配置生效
systemctl daemon-reload

# 重启Docker
systemctl restart docker
```
