---
title: 使用scp命令传输文件到服务器
date: 2022-02-15 15:34:58.699
updated: 2022-02-16 15:42:24.554
url: /archives/shi-yong-scp-ming-ling-chuan-shu-wen-jian-dao-fu-wu-qi
categories: 
- linux
tags: 
- scp
- 文件
---

# 使用scp命令

上传本地文件到服务器：

```bash
scp /path/filename username@servername:/path/
```

从服务器上下载文件：

```bash
scp username@servername:/path/filename /var/www/local_dir（本地目录）
```

从服务器下载整个目录：

```bash
scp -r username@servername:/var/www/remote_dir/（远程目录） /var/www/local_dir（本地目录）
```

上传目录到服务器：

```bash
scp -r local_dir username@servername:remote_dir
```