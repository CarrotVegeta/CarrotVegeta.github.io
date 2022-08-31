---
title: golang cmd命令执行
date: 2022-03-01 10:12:39.003
updated: 2022-03-01 10:12:39.003
url: /archives/golangcmd-ming-ling-zhi-xing
description: golang执行cmd命令.demo
categories: 
- golang
tags: 
- golang
- cmd
---


1.golang cmd命令执行
（１．）执行命令并获取执行结果
```go
package main

import (
	"fmt"
	"os/exec"
)

func main() {
	// 通过exec.Command函数执行命令或者shell
	// 第一个参数是命令路径，当然如果PATH路径可以搜索到命令，可以不用输入完整的路径
	// 第二到第N个参数是命令的参数
	// 下面语句等价于执行命令: ls -l /var/
	cmd := exec.Command("/bin/ls", "-l", "/var/")
	// 执行命令，并返回结果
	output,err := cmd.Output()
	if err != nil {
		panic(err)
	}
	// 因为结果是字节数组，需要转换成string
	fmt.Println(string(output))

}
```
（２．）执行命令不返回结果
```go
package main

import (
	"os/exec"
)

func main() {
	cmd := exec.Command("/bin/ls", "-l", "/var/")
	// 执行命令，返回命令是否执行成功
	err := cmd.Run()
	
	if err != nil {
		// 命令执行失败
		panic(err)
	}
}
```
