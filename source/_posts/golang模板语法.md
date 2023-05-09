---
abbrlink: ''
categories:
- - golang
date: '2023-05-09T20:57:20.261116+08:00'
excerpt: golang模板语法 package main  import (  &quot;bytes&quot;  &quot;encoding/json&quot;  &quot;log&quot;  &q...
tags:
- golang
- template
title: golang模板语法
updated: 2023-5-9T21:20:21.832+8:0
---
# golang模板语法

```go
package main

import (
	"bytes"
	"encoding/json"
	"log"
	"text/template"
)

type UserInfo struct {
	Name  string `json:"name"`
	Age   int    `json:"age"`
	Class struct {
		Name string `json:"name"`
	} `json:"class"`
}

func main() {
	//通过对象获取变量值
	a := "my name is {{.Name}}"
	user := UserInfo{
		Name: "li lei",
		Age:  18}
	funcs := template.FuncMap{
		"userinfo": GetUserInfo(),
	}
	tmpl, err := template.New("log").Parse(a)
	if err != nil {
		log.Fatalf(err.Error())
		return
	}
	writer := &bytes.Buffer{}
	//获取对象的变量值
	err = tmpl.Execute(writer, user)
	if err != nil {
		log.Fatalf(err.Error())
		return
	}
	//获取map里面的变量值
	a = ", age is {{.age}}"
	tmpl, err = tmpl.Parse(a)
	if err != nil {
		log.Fatalf(err.Error())
		return
	}
	var m map[string]any
	bs, _ := json.Marshal(user)
	json.Unmarshal(bs, &m)
	err = tmpl.Execute(writer, m)
	if err != nil {
		log.Fatalf(err.Error())
		return
	}
	//注册函数进去
	a = ", class is {{userinfo.Class.Name}}"
	tmpl, err = tmpl.Funcs(funcs).Parse(a)
	if err != nil {
		log.Fatalf(err.Error())
		return
	}
	err = tmpl.Execute(writer, nil)
	if err != nil {
		log.Fatalf(err.Error())
		return
	}
	log.Printf(writer.String())
}
func GetUserInfo() func() *UserInfo {
	return func() *UserInfo {
		return &UserInfo{
			Name: "li lei",
			Age:  18,
			Class: struct {
				Name string `json:"name"`
			}{Name: "五年级二班"},
		}
	}
}
```

输出：

```bash
my name is li lei, age is 18, class is 五年级二班
```
