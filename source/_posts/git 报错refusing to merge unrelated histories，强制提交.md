---
title: git 报错refusing to merge unrelated histories，强制提交
date: 2022-02-16 15:37:51.669
updated: 2022-02-16 15:37:51.669
url: /archives/gitbao-cuo-refusingtomergeunrelatedhistories-qiang-zhi-ti-jiao
categories: 
- git
tags: 
- git
---

拉取代码或者推送代码报错

```bash
refusing to merge unrelated histories
```

解决：  
方法一: 允许不相关历史提交，并强制合并

```bash
git pull origin master --allow-unrelated-histories
```

方法二： 强制提交

```bash
git push --force origin master
```