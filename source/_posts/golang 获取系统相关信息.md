---
title: golang 获取系统相关信息
url: /archives/golanghuo-qu-xi-tong-xiang-guan-xin-xi
categories:
  - golang
tags:
  - golang
abbrlink: '46454e21'
date: 2022-03-01 10:21:26
updated: 2022-03-01 10:21:26
---

```go
package main

import (
    "os"
    "runtime"
)

func main() {
    println(`系统类型：`, runtime.GOOS)

    println(`系统架构：`, runtime.GOARCH)

    println(`CPU 核数：`, runtime.GOMAXPROCS(0))

    name, err := os.Hostname()
    if err != nil {
        panic(err)
    }
    println(`电脑名称：`, name)
}
```