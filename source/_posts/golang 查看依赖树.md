---
abbrlink: ''
categories:
- - golang
date: '2023-12-12T17:05:00.214150+08:00'
excerpt: 安装 go get -u github.com/PaulXu-cn/go-mod-graph-chart/gmchart  检查安装情况 gmchart --help  Usage of ~\go\b...
tags:
- golang
title: golang 查看依赖树
updated: '2023-12-12T17:07:58.951+08:00'
---
## 安装

```bash
go get -u github.com/PaulXu-cn/go-mod-graph-chart/gmchart
```

检查安装情况

```bash
gmchart --help

Usage of ~\go\bin\gmchart:
  -debug int
        is debug model
  -keep int
        start http server not exit
```

进入项目，执行命令

```bash
go mod graph | gmchart
```
