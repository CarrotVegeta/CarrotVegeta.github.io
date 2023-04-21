---
title: git 配置用户名邮箱
date: 2022-02-16 15:36:50.932
updated: 2022-02-16 15:36:50.932
url: /archives/gitpei-zhi-yong-hu-ming-you-xiang
categories: 
- git
tags: 
- git
---

## git全局用户名邮箱配置

```bash
git config --global user.name  "username"  
git config --global user.email  "email"
```

## git局部用户名邮箱配置

```bash
git config  user.name  "username"  
git config  user.email  "email"
```

## 修改已有配置信息

```bash
git config --replace-all user.name "name"

git config --replace-all user.email "123@qq.com"
```

### 注意：局部变量覆盖全局变量！！！