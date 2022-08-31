---
title: channel的死锁和panic总结
date: 2022-05-07 16:30:27.778
updated: 2022-05-07 17:17:18.964
url: /archives/channel的死锁和panic总结
description: channel的死锁和panic总结
categories: 
- golang
tags: 
- golang
---

# channel的死锁和panic总结

## 1.channel的容量为0时

### 第一种情况
```go
func main() {
	c := make(chan int, 0)
     //一直接收不到消息发生阻塞，死锁
	fmt.Println(<-c)
}
```
### 第二种情况
```go
func main() {
	c := make(chan int, 0)
    //发送消息的时候无人接收
	c <- 1
    //接收消息 的时候无人发送 死锁
	fmt.Println(<-c)
}
```
这是因为channel size为0，之前把1传进c中，但是没有接收方，等到<-c时，已经接收不到数据传入channel，所以死锁

### 第三种情况
```go
func main() {
   //没有初始化
	var ch chan int
	go func() {
		ch <- 1
	}()
	fmt.Println(<-ch)
}
```

### 第四种情况

```go

func demo5() {
    var c chan int //定义类型
    c = make(chan int ,0) //初始化
    go func() {
        for i := 0; i < 3; i++ {
            c <- i //传入channel数据
        }
    }()
    for  v := range c{
        fmt.Println(v)
    }
}
```
结果是死锁，应该将数据传进channel后，并没有关闭channel，for循环接收channel一直在监听，死锁

数据传进channel后，输入方主动关闭channel
```go
func demo5() {
    var c chan int
    c = make(chan int ,0)
    go func() {
        for i := 0; i < 3; i++ {
            c <- i
        }
        close(c) //或者defer close(c)
    }()
    for  v := range c{
        fmt.Println(v)
    }
}
```
### 当channel 关闭且缓冲区为0时
```go
func demo6() {
    c := make(chan int,1)
    close(c)
    fmt.Println(<-c)
}

//输出为0
```
### 关闭未初始化的channel，会panic
```go
func demo6() {
    var c chan int
    close(c)
}
```

### 特殊情况
```go
func main() {
    var ch chan int
    //func1
    go func() {
        ch = make(chan int, 1)
        ch <- 1
    }()
    //func2
    go func(ch chan int) {
        time.Sleep(time.Second)
        <-ch
    }(ch)
    c := time.Tick(1 * time.Second)
    for range c {
        fmt.Printf("#goroutines: %d\n", runtime.NumGoroutine())
    }
}

//一段时间后输出结果为#goroutines: 2
```
结果中的goroutines分别为fun2 和 main函数，因为fun2没有初始化ch 所以会一直阻塞（func1中的初始化只在其所在的闭包函数中有效)

### 注意点：
1. channel关闭后，再向channel中写入数据会panic
2. channel关闭后，再次关闭channel，会panic
3. 关闭未初始化的channel，会panic
4. channel关闭后，可以继续从channel中接收数据
5. 当channel 关闭且缓冲区为0时，继续从channel接收数据会接收到一个channel定义类型的零值
6. channel先进先出

作者：coldwarm7
链接：https://www.jianshu.com/p/f25cdd72efce
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
