---
title: git 文件状态 、status
url: /archives/gitwen-jian-zhuang-tai-status
categories:
  - git
tags:
  - git
  - 文件
  - status
abbrlink: 56d54cc1
date: 2022-02-16 15:44:04
updated: 2022-02-16 15:51:13
---

# git status命令

查看文件更改状态

```bash
git status
```

带文件状态码查看

```bash
git status -s
```

- **A**: 你本地新增的文件（服务器上没有）.
  
- **C**: 文件的一个新拷贝.
  
- **D**: 你本地删除的文件（服务器上还在）.
  
- **M**: 文件的内容或者mode被修改了.
  
- **R**: 文件名被修改了。
  
- **T**: 文件的类型被修改了。
  
- **U**: 文件没有被合并(你需要完成合并才能进行提交)。
  
- **X**: 未知状态(很可能是遇到git的bug了，你可以向git提交bug report)
  
- **?**：未被git进行管理，可以使用git add file1把file1添加进git能被git所进行管理