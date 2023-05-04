---
title: go mod tidy 报错timeout
url: /archives/gomodtidy报错timeout
categories:
  - golang
tags:
  - golang
abbrlink: 394fab29
date: 2022-05-05 16:33:04
updated: 2022-05-05 16:39:43
---

# go mod tidy 报错timeout
当我们设置了GOPROXY代理之后
```bash
go env -w GOPROXY=https://goproxy.io,direct
```
或者
```bash
go env -w GOPROXY=https://goproxy.cn,direct
```
执行 go mod tidy 命令报错
```bash
go mod tidy
github.com/spf13/viper: github.com/spf13/viper@v1.11.0: verifying module: github.com/spf13/viper@v1.11.0: Get "https://sum.golang.org/lookup/github.com/spf13/viper@v1.11.0": dial tcp 142.251.42.241:443: i/o timeout
```
报错是因为更改了GOPROXY 导致校验不通过
解决办法
1.关闭GOSUMDB 校验
```bash
go env -w GOSUMDB=off
```
2.设置另一个国内可用的sum验证服务
```bash
go env -w GOSUMDB="sum.golang.google.cn"
```
