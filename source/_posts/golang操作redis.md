---
title: golang 操作redis
url: /archives/golangcao-zuo-redis
categories:
  - golang
tags:
  - golang
  - redis
abbrlink: 7e553d6e
date: 2022-02-21 12:29:11
updated: 2022-02-21 12:54:07
---
golang操作redis库
<!--more-->
```go
package main

import (
	"context"
	"fmt"
	"github.com/go-redis/redis/v8"
	"log"
)

var rdb *redis.Client

func main() {
	OpenDB()
	SetKeyValue("key", "value")
	v1, err := GetValueByValue("key")
	if err != nil {
		log.Fatal(err.Error())
	}
	fmt.Println("v1:", v1)

	v2, err := GetValueByValue("key")
	if err != nil {
		log.Fatal(err.Error())
	}
	fmt.Println("v2:", v2)
}
func OpenDB() {
	rdb = redis.NewClient(&redis.Options{
		Addr:     "localhost:6379",
		Password: "", // no password set
		DB:       0,  // use default DB
	})

}
func SetKeyValue(key, value string) {
	err := rdb.Set(context.Background(), key, value, 0).Err()
	if err != nil {
		log.Fatal(err)
	}
}
func GetValueByValue(key string) (string, error) {
	v, err := rdb.Get(context.Background(), key).Result()
	if err != nil {
		log.Fatal(err)
		return "", err
	}
	return v, nil
}


```
