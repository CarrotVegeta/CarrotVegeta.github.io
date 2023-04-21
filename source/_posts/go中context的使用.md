---
title: go 中context的使用
date: 2022-05-07 15:28:52.002
updated: 2022-05-07 15:38:13.718
url: /archives/go中context的使用
description: 
categories: 
- golang
tags: 
- golang
---

# go中context的使用
版权声明：本文为CSDN博主「Word哥」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/finghting321/article/details/106012673/
————————————————

## 1. 为什么需要context
在并发程序中，由于超时、取消操作或者一些异常情况，往往需要进行抢占操作或者中断后续操作。

举个例子：在 Go http包的Server中，每一个请求在都有一个对应的 goroutine 去处理。请求处理函数通常会启动额外的 goroutine 用来访问后端服务，比如数据库和RPC服务，用来处理一个请求的 goroutine 通常需要访问一些与请求特定的数据，比如终端用户的身份认证信息、验证相关的token、请求的截止时间。 当一个请求被取消或超时时，所有用来处理该请求的 goroutine 都应该迅速中断退出，然后系统才能释放这些 goroutine 占用的资源。context深入理解可参考
<!--more-->
context常用的使用场景：
1. 一个请求对应多个goroutine之间的数据交互
2. 超时控制
3. 上下文控制

## 2. context包简介
context.Context接口：
```go
type Context interface {
    // 返回Context的超时时间（超时返回场景）
    Deadline() (deadline time.Time, ok bool)
 
    // 在Context超时或取消时（即结束了）返回一个关闭的channel
    // 即如果当前Context超时或取消时，Done方法会返回一个channel，然后其他地方就可以通过判断Done方法是否有返回（channel），如果有则说明Context已结束
    // 故其可以作为广播通知其他相关方本Context已结束，请做相关处理。
    Done() <-chan struct{}
 
    // 返回Context取消的原因
    Err() error
    
    // 返回Context相关数据
    Value(key interface{}) interface{}
}
```
继承的Context，BackGound是所有Context的root，不能够被cancel。context包提供了三种context，分别是是普通context，超时context以及带值的context：
```go
// 普通context，通常这样调用： ctx, cancel := context.WithCancel(context.Background())
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
 
// 带超时的context，超时之后会自动close对象的Done，与调用CancelFunc的效果一样
// WithDeadline 明确地设置一个d指定的系统时钟时间，如果超过就触发超时
// WithTimeout 设置一个相对的超时时间，也就是deadline设为timeout加上当前的系统时间
// 因为两者事实上都依赖于系统时钟，所以可能存在微小的误差，所以官方不推荐把超时间隔设置得太小
// 通常这样调用：ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
func WithDeadline(parent Context, d time.Time) (Context, CancelFunc)
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
 
// 带有值的context，没有CancelFunc，所以它只用于值的多goroutine传递和共享
// 通常这样调用：ctx := context.WithValue(context.Background(), "key", myValue)
func WithValue(parent Context, key, val interface{}) Context
```
## 3. 场景举例—等待组
```go
package main
 
import (
    "fmt"
    "sync"
    "time"
)
 
//数据接收服务主协程同子协程同步变量
var wg sync.WaitGroup
 
func run(i int) {
    fmt.Println("start 任务ID：", i)
    time.Sleep(time.Second * 1)
    wg.Done() // 每个goroutine运行完毕后就释放等待组的计数器
}
 
func main() {
    countThread := 2 //runtime.NumCPU()
    for i := 0; i < countThread; i++ {
        go run(i)
    }
    wg.Add(countThread) // 需要开启的goroutine等待组的计数器
 
    //等待所有的任务都释放
    wg.Wait()
    fmt.Println("任务全部结束,退出")
}
```
**运行结果：** 

<img align="left" src="http://rd7bcspti.hn-bkt.clouddn.com/02.png">  

**分析：**  对于等待组控制多并发的情况，只有所有的goroutine都结束了才算结束，只要有一个goroutine没有结束， 那么就会一直等，这显然对资源的释放是缓慢的；
**优点：** 使用等待组的并发控制模型，适用于好多个goroutine协同做一件事情，因为每个goroutine做的都是这件事情的一部分，只有当全部的goroutine都完成，这件事情才算完成；
**缺点：** 需要主动的通知某一个 goroutine 结束。
**疑问：** 如果开启一个后台 goroutine 一直做事情，现在不需要了，那么就需要通知这个goroutine 结束，否则它会一直跑。

## 4. 场景举例—通道+select
针对等待组场景遗留的问题，解决办法：
> 1. 设置全局变量，在通知goroutine要停止时，为全局变量赋值，但是这样必须保证线程安全，不可避免的必须为全局变量加锁，显得有失便利；
> 2. 使用chan + select多路复用的方式，就会优雅许多；
```go
package main
 
import (
    "fmt"
    "time"
)
 
func run(stop chan bool) {
    for {
        select {
        case <-stop:
            fmt.Println("任务1结束退出")
            return
        default:
            fmt.Println("任务1正在运行中")
            time.Sleep(time.Second * 2)
        }
    }
}
 
func main() {
    stop := make(chan bool)
    go run(stop) // 开启goroutine
 
    // 运行一段时间后停止
    time.Sleep(time.Second * 10)
    fmt.Println("停止任务1。。。")
    stop <- true
    time.Sleep(time.Second * 3)
    return
}
```
**运行结果：** 
![image-1651908425418](/upload/2022/05/image-1651908425418.png)
**优点：** 优雅、简单
**不足：** 如果有很多 goroutine 都需要控制结束，并且这些 goroutine 又开启其它更多的goroutine ？

## 5. 场景举例—普通context
```go
package main
 
import (
    "context"
    "fmt"
    "time"
)
 
func run(ctx context.Context, id int) {
    for {
        select {
        case <-ctx.Done():
            fmt.Printf("任务%v结束退出\n", id)
            return
        default:
            fmt.Printf("任务%v正在运行中\n", id)
            time.Sleep(time.Second * 2)
        }
    }
}
 
func main() {
    //管理启动的协程
    ctx, cancel := context.WithCancel(context.Background())
    // 开启多个goroutine，传入ctx
    go run(ctx, 1)
    go run(ctx, 2)
    // 运行一段时间后停止
    time.Sleep(time.Second * 10)
    fmt.Println("停止任务1")
    cancel() // 使用context的cancel函数停止goroutine
    // 为了检测监控过是否停止，如果没有监控输出，表示停止
    time.Sleep(time.Second * 3)
    return
}
```
**说明：** context.Background() 返回一个空的 Context，这个空的 Context 一般用于整个 Context 树的根节点。然后使用 context.WithCancel(parent) 函数，创建一个可取消的子 Context，然后当作参数传给 goroutine 使用，这样就可以使用这个子 Context 跟踪这个 goroutine。

**运行结果：**
![image-1651908618179](/upload/2022/05/image-1651908618179.png)

## 6. 场景举例—Context超时
```go
package main
 
import (
    "context"
    "fmt"
    "sync"
    "time"
)
 
func coroutine(ctx context.Context, duration time.Duration, id int, wg *sync.WaitGroup) {
    for {
        select {
        case <-ctx.Done():
            fmt.Printf("协程 %d 退出\n", id)
            wg.Done()
            return
        case <-time.After(duration):
            fmt.Printf("消息来自协程 %d\n", id)
        }
    }
}
 
func main() {
    //使用WaitGroup等待所有的goroutine执行完毕，在收到<-ctx.Done()的终止信号后使wg中需要等待的goroutine数量减一。
    // 因为context只负责取消goroutine，不负责等待goroutine运行，所以需要配合一点辅助手段
    //管理启动的协程
    wg := &sync.WaitGroup{}
    ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
    defer cancel()
    for i := 0; i < 3; i++ {
        wg.Add(1)
        go coroutine(ctx, 1*time.Second, i, wg)
    }
    wg.Wait()
 }
```
**说明：** 代码中使用WaitGroup等待所有的goroutine执行完毕，在收到<-ctx.Done()的终止信号后使wg中需要等待的goroutine数量减一， 因为context只负责取消goroutine，不负责等待goroutine运行，需要配合一点辅助手段
**运行结果：** 

![image-1651908781361](/upload/2022/05/image-1651908781361.png)

## 7. 场景举例—Context传递元数据
```go
package main
 
import (
    "context"
    "fmt"
    "time"
)
 
var key string = "name"
 
func run(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            fmt.Printf("任务%v结束退出\n", ctx.Value(key))
            return
        default:
            fmt.Printf("任务%v正在运行中\n", ctx.Value(key))
            time.Sleep(time.Second * 2)
        }
    }
}
 
func main() {
    //管理启动的协程
    ctx, cancel := context.WithCancel(context.Background())
    // 给ctx绑定键值，传递给goroutine
    valuectx := context.WithValue(ctx, key, "【监控1】")
    // 开启goroutine，传入ctx
    go run(valuectx)
    // 运行一段时间后停止
    time.Sleep(time.Second * 10)
    fmt.Println("停止任务")
    cancel() // 使用context的cancel函数停止goroutine
    // 为了检测监控过是否停止，如果没有监控输出，表示停止
    time.Sleep(time.Second * 3)
}
```
**运行结果：** 
![image-1651908878817](/upload/2022/05/image-1651908878817.png)

## 8. context总结

> 1. 不要把 Context 放在结构体中，要以参数的方式传递
> 2. 以 Context 作为参数的函数方法，应该把 Context 作为第一个参数，放在第一位
>3. 给一个函数方法传递 Context 的时候，不要传递 nil，如果不知道传递什么，就使用 context.TODO
>4. Context 的 Value 相关方法应该传递必须的数据，不要什么数据都使用这个传递
>5. Context 是线程安全的，可以放心的在多个 goroutine 中传递
