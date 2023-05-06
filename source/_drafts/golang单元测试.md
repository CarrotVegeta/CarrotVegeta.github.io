---
abbrlink: ''
categories:
- - golang
date: '2023-05-07T00:23:52.951300+08:00'
tags:
- golang
title: golang单元测试
updated: 2023-5-7T0:23:53.597+8:0
---
# golang单元测试

golang单元测试文件以_test.go 结尾

```go
go/
 -user_test.go

```

- 测试用例名称一般命名为Test加上待测试的方法名
- 测试的参数有且只有一个，一般是 t *testing.T
- 基准测试的参数是 *testing.B  TestMain的参数是 *testing.M类型

```go
func TestUser_FindByName(t *testing.T) {
	//initDB()
	//u := User{}
	fmt.Println("sdkfjsdkfjds")
	//u.FindByName(DB, "sdk").Find()
}
func TestUser_CalculateAge(t *testing.T) {
	initDB()
	CreateUser()
	var users []struct {
		ID     uint   `json:"id"`
		Name   string `json:"name"`
		AgeNow int    `json:"age_now"`
	}
	err := DB.Model(&User{}).Select("id,name,CalculateAge() as age_now").Find(&users).Error
	if err != nil {
		log.Fatalf(err.Error())
		return
	}
	log.Println(users)
}

```

运行测试，该package目录下的所有测试用例都会yunx

```bash
go test
```

- -v:显示每个参数的测试结果
- -cover:显示覆盖结果

如果只想运行其中一个用例

```bash
go test -v -run TestUser_FindByName
```
