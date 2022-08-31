---
title: golang 使用pprof 进行性能分析
date: 2022-02-16 15:58:29.707
updated: 2022-02-16 15:58:29.707
url: /archives/golangshi-yong-pprofjin-xing-xing-neng-fen-xi
categories: 
- golang
tags: 
- pprof
- golang
---

# golang 性能分析

## 性能分析web地址

http://ip:port/debug/pprof/
<!--more-->
## go tool 命令

top 排序

list 列出调用栈

## 内存分析

- -insue_space 生成当前程序内存占用图
- --alloc_space 生成历史内存占用图

### 命令行生成svg图片分析

- go tool pprof -inuse_space -cum -svg http://ip:port/debug/pprof/heap > heap_inuse1.svg

## cpu占用分析

### 命令行分析

- go tool pprof im_gate cpu.prof # im_gate 是程序名字

### 生成图片分析

- go tool pprof -png http://127.0.0.1:8888/debug/pprof/profile > cpu.png
