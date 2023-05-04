---
title: git 配置http代理
url: /archives/gitpei-zhi-http-dai-li
categories:
  - git
tags:
  - git
  - 代理
abbrlink: 34f1116b
date: 2022-02-16 15:40:39
updated: 2022-02-16 15:41:34
---

设置代理

```bash
git config --global https.proxy http://127.0.0.1:1080  
git config --global https.proxy https://127.0.0.1:1080
```

取消代理

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```