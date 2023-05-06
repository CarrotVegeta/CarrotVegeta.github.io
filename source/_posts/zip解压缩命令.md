---
abbrlink: ''
categories:
- - 日常命令
date: '2023-05-06T22:45:45.279717+08:00'
excerpt: zip解压缩命令 zip语法: zip [-r] 参数1 参数2 ...参数N  -r,被压缩的包含文件夹的时候，需要使用-r选项，和rm、cp等命令的-r效果一致  示例：  zip text.zi...
tags:
- zip
title: zip解压缩命令
updated: 2023-5-6T23:6:26.650+8:0
---
# zip解压缩命令

**zip语法:**

zip [-r] 参数1 参数2 ...参数N

- -r,被压缩的包含文件夹的时候，需要使用-r选项，和rm、cp等命令的-r效果一致

示例：

- zip text.zip a.txt b.txt c.txt

将a.txt b.txt c.txt压缩到text.zip文件夹内

- zip -r text.zip test a.txt

将test a.txt压缩到test.zip文件夹内

**unzip语法:**

unzip [-d] 参数

- -d,指定要解压的位置
- 参数，被解压的zip压缩包文件

示例:

- unzip text.zip,将text.zip解压到当前目录
- unzip text.zip -d /home/text,将text解压到/home/text
