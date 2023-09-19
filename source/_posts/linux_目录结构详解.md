---
abbrlink: linux_目录结构详解
categories:
- - linux
date: '2023-09-19T22:35:19.211796+08:00'
excerpt: 具体的目录结构  /bin （/usr/bin、/usr/local/bin） 是Binary的缩写，这个目录存放着最经常使用的命令 /sbin (/usr/sbin、/usr/local/sbin)...
tags:
- linux
title: linux_目录结构详解
updated: '2023-09-19T22:50:56.880+08:00'
---
## 具体的目录结构

- /bin （/usr/bin、/usr/local/bin）
  是Binary的缩写，这个目录存放着最经常使用的命令
- /sbin (/usr/sbin、/usr/local/sbin)
  s就是super user的意思，这里存放的是系统管理员使用的系统管理程序
- /home
  存放普通用户的主目录，在linux中每个用户都有一个自己的目录，一般用该目录名是以用户的账号命名
- /root
  该目录为系统管理员，也称作超级权限者的用户主目录
- /lib
  系统开机所需要的动态连接共享库，其作用类似于windows里的DLL文件
- /lost+found
  当系统非法关机后，这里就存放了一些文件
- /etc
  所有的系统管理所需要的配置文件和子目录，比如安装mysql数据库 my.conf
- /usr
  用户的很多应用程序和文件都放这个目录下，类似windows下的program files目录
- /boot
  存放的是启动linux时使用的一些核心文件，包括一些连接文件以及镜像文件
- /tmp
  这个目录是用来存放一些临时文件的
- /dev
  类似于windows的设备管理器，把所有的硬件用文件的形式存储
- /media
  linux系统会自动识别一些设备，例如u盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下
- /mnt
  系统提供该目录是为了挂载别的文件系统的，
- /opt
  这是给主机额外安装软件的目录
- /usr/local
  这是另一个给主机额外安装软件的目录，一般是通过编译源码方式安装的程序
- /var
  这个目录中存放着在不断扩充的东西，习惯将经常被修改的目录放在这个目录下，包括各种日志文件
