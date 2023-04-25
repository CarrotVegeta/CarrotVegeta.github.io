---
abbrlink: ''
categories:
- - linux
date: '2023-04-25 23:23:32'
tags:
- linux
title: linux查看进程
updated: Tue, 25 Apr 2023 15:23:36 GMT
---
# linux查看进程

ps [-e -f]

- 选项：-e 显示全部进程
- 选项：-f 以完全格式化的形式展示信息

```bash
ps -ef
```

![进程展示](http://img.carrotvegeta.icu/Snipaste\_2023-04-25\_23-28-13.png)

- UID:进程所属的用户ID
- PID:进程的进程号ID
- PPID:进程所属的父ID（启动此进程的其他进程）
- C:此进程的CPU占用率（百分比）
- STIME:进程的启动时间
- TTY:启动此进程的终端序号，如果显示？ 则表示不是由终端启动
- TIME:进程所占用的cpu时间
- CMD:启动此进程的命令或启动路径
