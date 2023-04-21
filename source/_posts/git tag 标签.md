---
title: git 标签
date: 2022-02-16 15:43:25.337
updated: 2022-02-16 15:44:17.602
url: /archives/gitbiao-qian
categories: 
- git
tags: 
- git
- tag
- 标签
---

# git tag

## 打标签

```bash
git tag -a v1.2.0 -m "new version"
```

## 显示标签以及备注

```bash
git tag -n
```

## 实现标签以及备注，按照打标签的时间排序

```bash
git tag -n --sort=taggerdate
```

--sort=key

关于key的值可以参考 [https://git-scm.com/docs/git-for-each-ref](https://git-scm.com/docs/git-for-each-ref)

## 同时有时间、tag、备注使用以下命令

```bash
git for-each-ref --sort=taggerdate --format '%(refname:short) %(taggerdate:short) %(subject)'
```

## 对某个版本进行补打标签，commitId 只要填入前7位即可

```bash
git tag -a v1.2 423445a
```

## 删除标签，并不会删除版本，只是删除标签

```bash
git tag -d v1.2
```

## 推送标签

```bash
git push origin master --tags
```