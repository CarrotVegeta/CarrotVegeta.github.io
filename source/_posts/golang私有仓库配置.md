title: golang私有仓库配置
date: 2022-08-23 15:19:20
updated: 2022-08-23 15:19:20
categories:

- golang 

tags:

- golang
---
golang设置
```golang
go env -w GOPRIVATE="gitlab.xxx.com/xxxx/*"
```
git设置
```bash
git config --global url."git@gitlab.xxxx.com:".insteadOf "http://gitlab.xxxx.com/"
```
