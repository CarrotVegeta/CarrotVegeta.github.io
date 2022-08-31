---
title: git 设置ssh 代理
date: 2022-02-16 15:41:21.313
updated: 2022-02-16 15:41:44.62
url: /archives/gitshe-zhi-sshdai-li
categories: 
- git
tags: 
- git
- 代理
---

配置文件，如果不存在则自行创建一个

```bash
vim ~\.ssh\config
```

增加内容,端口号设置socks端口号

```bash
ProxyCommand connect -S 127.0.0.1:10808 -a none %h %p

Host github.com
  User git
  Port 22
  Hostname github.com
  # 注意修改路径为你的路径
  IdentityFile "~\.ssh\id_rsa"
  TCPKeepAlive yes

Host ssh.github.com
  User git
  Port 443
  Hostname ssh.github.com
  # 注意修改路径为你的路径
  IdentityFile "~\.ssh\id_rsa"
  TCPKeepAlive yes
```