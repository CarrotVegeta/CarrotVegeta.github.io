---
title: golang 换行读写
url: /archives/golanghuan-xing-du-xie
description: golang换行读写示例代码
categories:
  - golang
tags:
  - golang
abbrlink: f906baad
date: 2022-03-02 16:19:40
updated: 2022-03-02 16:19:40
---

```go
package utils

import (
	"bufio"
	"bytes"
	"fmt"
	"io"
	"os"
	"strings"
)
func ReadLines(path string) (lines []string, err error) {
	var (
		file   *os.File
		part   []byte
		prefix bool
	)

	if file, err = os.Open(path); err != nil {
		return
	}

	reader := bufio.NewReader(file)
	buffer := bytes.NewBuffer(make([]byte, 1024))

	for {
		if part, prefix, err = reader.ReadLine(); err != nil {
			break
		}
		buffer.Write(part)
		if !prefix {
			lines = append(lines, buffer.String())
			buffer.Reset()
		}
	}
	if err == io.EOF {
		err = nil
	}
	return
}

func WriteLines(lines []string, path string) (err error) {
	var file *os.File

	if file, err = os.Create(path); err != nil {
		return
	}

	defer file.Close()

	for _, elem := range lines {
		_, err := file.WriteString(strings.TrimSpace(elem) + "\n")
		if err != nil {
			fmt.Println(err)
			break
		}
	}
	return
}
```
