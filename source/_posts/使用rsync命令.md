---
title: 使用rsync命令
date: 2022-02-16 15:54:19.532
updated: 2022-02-16 15:54:19.532
url: /archives/shi-yong-rsync-ming-ling
categories: 
- linux
tags: 
- linux
- rsync
---

# 本地上传至远程
```bash
rsync -av localPath username@ip:remotepath
```

-  --progress 参数 可显示进度条

-  -azvrtopg 增量更新
-   --exclude='path' 忽略文件夹或某个文件