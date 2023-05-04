---
title: docker 拉取指定平台架构的镜像
url: /archives/dockerla-qu-zhi-ding-ping-tai-de-jing-xiang
categories:
  - docker
tags:
  - docker
abbrlink: bd1646ec
date: 2022-02-16 15:34:57
updated: 2022-03-31 20:07:53
---

# docker 拉取指定平台架构的镜像
有时候我们需要拉取指定平台架构的镜像但是我们又没有对应架构的服务器和硬件的时候：
比如我们需要拉取一个arm64的node镜像，首先进入docker hub 搜索node镜像
![image.png](/upload/2022/03/image-f8364fe234a64afe9a6c1d20ee65a75e.png)
复制digest
![image.png](/upload/2022/03/image-38e509972a8f4187982b3a5922ff0072.png)
拉取对应版本和对应架构的镜像
```bash
docker pull nginx:latest@sha256:3df2ee4220cd7ce126d98cad124c93ed81ea58ee050400ec3c3bca3b553d5448
```
在DOCKERFILE文件里面
```dockerfile
FROM nginx:latest@sha256:3df2ee4220cd7ce126d98cad124c93ed81ea58ee050400ec3c3bca3b553d5448
```
成功拉取