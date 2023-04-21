---
title: golang 用两个栈实现队列
date: 2022-03-15 11:09:43.187
updated: 2022-03-15 11:09:43.187
url: /archives/golangyong-liang-ge-zhan-shi-xian-dui-lie
description: golang用栈实现队列代码
categories: 
- golang
tags: 
- golang
---

```go
package main


import (
	"container/list"
	"fmt"
)

func main() {
	c := Constructor()
	c.AppendTail(3)
	e := c.DeleteHead()
	fmt.Println(e)
	e = c.DeleteHead()
	fmt.Println(e)
	e = c.DeleteHead()
	fmt.Println(e)
}

type CQueue struct {
	stack1, stack2 *list.List
}

func Constructor() CQueue {
	return CQueue{
		stack1: list.New(),
		stack2: list.New(),
	}
}

func (this *CQueue) AppendTail(value int) {
	this.stack1.PushBack(value)
}

func (this *CQueue) DeleteHead() int {
	if this.stack2.Len() == 0 {
		for this.stack1.Len() > 0 {
			this.stack2.PushBack(this.stack1.Remove(this.stack1.Back()))
		}
	}
	if this.stack2.Len() != 0 {
		return this.stack2.Remove(this.stack2.Back()).(int)
	}
	return -1
}

/**
* Your CQueue object will be instantiated and called as such:
* obj := Constructor();
* obj.AppendTail(value);
* param_2 := obj.DeleteHead();
*/
```
