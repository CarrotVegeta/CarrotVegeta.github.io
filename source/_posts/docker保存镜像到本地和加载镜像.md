---
title: docker 保存镜像到本地和加载镜像
url: /archives/docker保存镜像到本地和加载镜像
description: docker保存镜像到本地以及加载镜像
categories:
  - docker
tags:
  - docker
abbrlink: a1015970
date: 2022-03-30 14:20:55
updated: 2022-03-30 14:32:57
---

# docker 保存镜像到本地和加载镜像

## 导出docker镜像，到linux本地

```bash   
docker save -o   指定地址和文件名   镜像名 
```

- 例子：  
把名字为test，版本为4.0的docker镜像，保存到/data/export目录下，保存名字和格式为test.tar
```bash 
docker save -o /data/export/test.tar test:4.0
```

## 加载镜像文件

```bash
docker load < 文件名
```

