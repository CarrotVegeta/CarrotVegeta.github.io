---
title: clickhouse安装使用笔记
date: 2022-12-14 20:17:27.778
updated: 2022-12-14 20:17:27.778
description: clickhouse 笔记
categories: 
- clickhouse
tags: 
- clickhouse
---
# clickhouse

## clickhouse入门

clickhouse 的特点：

-  列式存储

- DBMS的功能
- 多样化引擎 （merge_tree)
- 高吞吐写入能力
- 数据分区与线程级并行
- 性能-不适合用join查询

##  安装

**ClickHouse安装准备**

- 确定防火墙关闭
- Centos取消打开文件限制->用户可用文件数、用户可用最大进程数
- 安装依赖
- CentOS取消SELINUX

 - 查看selinux状态

```shell
getenforce
```

修改/etc/selinux/config中的SELINUX=disabled

```shell
sudo vim /etc/selinux/config
SELINUX=disabled
```

重启生效

如果不想关闭机器，临时生效，如果是关闭状态必须重启机器

```shell
setenforce 0
```

 执行同步操作

```shell
sudo /home/user/bin/xsync/etc/selinux/config
```

**下载安装包**

- client  
- common-static 
- common-static-dbg
- server 

**版本差异**

- 20.5 支持多线程
- 20.6.3+ explain(执行计划)
- 20.8 -> 实时同步mysql新引擎

**安装命令**

```shell
mkdir clickhouse
cd clickhouse
sudo rpm -ivh *.rpm
#输入密码
enter password for default user:******
#确认安装状态
rpm -qa|grep clickhouse
#显示有四个安装包则成功
clickhouse-server.noarch
click-client.noarch
click-common-static-dbg
click-common-static
```
## clickhouse操作

查看目录

```shell
#bin目录
cd /usr/bin/
#conf目录
cd /etc/clickhouse-server/
#lib目录
cd /var/lib/clickhouse
#log目录
cd /var/log/clickhouse
```
查看配置
```shell
#进入clickhouse文件夹
cd /etc/click-server/
ls
#config.xml 通用服务端配置，可以修改数据路径和日志路径
config.d config.xml
#users.xml 用户一些参数配置
users.d users.xml
 
vim config.xml 
查找listen,去掉注释，不对ip做限制
<lisetn_host>::</listen_host> 
```
clickhouse服务相关命令
```shell
#启动server
#linux 查看状态
sudo systemctl status clickhouse-server
#clickhouse 自带命令查看状态
sudo clickhouse status
#重启
sudo clickhouse restart
#clickhouse-client 命令
clickhouse-client -help
-m 分号换行
-h 链接远程
--query "加上查询语句" clickhouse-client --query "show databases"
-p 端口
```
clickhouse数据库操作命令
```shell
#链接clickhouse
clickhouse -m
#查看库
show databases;
# 使用库
use system;
#查看表
show tables;
#查询
select * from users;
```

