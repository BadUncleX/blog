---
layout: post
title:  " 100天第五天 golang goroutine and channel"
date:   2017-01-08 20:34:24 +0800
categories: golang 100days goroutine channel
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}


## goroutines

A *goroutine* is a lightweight thread managed by the Go runtime.



Goroutines can be thought of as light weight threads. The cost of creating a Goroutine is tiny when compared to a thread. Hence its common for Go applications to have thousands of Goroutines running concurrently.





### Advantages of Goroutines over threads

- Goroutines are extremely cheap when compared to threads. They are only a few kb in stack size and the stack can grow and shrink according to needs of the application whereas in the case of threads the stack size has to be specified and is fixed. 

- > 消耗资源极少

- The Goroutines are multiplexed to fewer number of OS threads. There might be only one thread in a program with thousands of Goroutines. If any Goroutine in that thread blocks say waiting for user input, then another OS thread is created and the remaining Goroutines are moved to the new OS thread. All these are taken care by the runtime and we as programmers are abstracted from these intricate details and are given a clean API to work with concurrency.

- > 多路复用技术, 即有阻塞的时候剩余Goroutine会转移到新的thread

- Goroutines communicate using channels. Channels by design prevent race conditions from happening when accessing shared memory using Goroutines. Channels can be thought of as a pipe using which Goroutines communicate. We will discuss channels in detail in the next tutorial.

- > 借助channel通讯, channel的好处是应对使用goroutine时访问共享内存出现的竞争条件

```go
func say(s string) {
   for i := 0; i < 5; i++ {
      time.Sleep(100 * time.Millisecond)
      fmt.Println(s)
   }
   
}

func main()  {
   go say("word") //新启一个线程
   say("hello")
}
```

output:

word
hello
word
hello
word
hello
word
hello
word
hello



### 协程 coroutine

- 轻量级线程
- 非抢占式多任务处理, 由协程主动交出控制权
- 编译器/解释器/虚拟机层面的多任务
- 多个协程可能在一个或多个线程上运行





### 理解非抢占式

```go
//执行后程序无法结束
func goroutinecontrol()  {
   var a [10]int
   for i := 0 ; i < 10; i++ {
      go func(i int) {
         for  {
            //没有机会交出控制权
            a[i] ++
         }
      }(i)
   }

   time.Sleep(time.Millisecond)
   fmt.Println(a)
   
}
```



可以手动交出控制权

```go
func goroutinecontrol()  {
   var a [10]int
   for i := 0 ; i < 10; i++ {
      go func(i int) {
         for  {
            a[i] ++
            runtime.Gosched() //手动交出控制权
         }
      }(i)
   }

   time.Sleep(time.Millisecond)
   fmt.Println(a)
   
}
```



### 子程序是协程的一个特例

![](/assets/images/2018-07/2018-07-05-223355.png)



### goroutine的定义

![](/assets/images/2018-07/2018-07-05-223841.png)





### goroutine可能的切换点

![](/assets/images/2018-07/2018-07-05-224017.png)



## Channels

channel的理论基础: CSP  Communication Sequential Process

![image-20180706073710768](/assets/images/2018-07/image-20180706073710768.png)

> channel是goroutine和goroutine之间的交互



Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`.



Like maps and slices, channels must be created before use:

```go
ch := make(chan int)
```

By default, sends and receives block until the other side is ready. This allows goroutines to **synchronize** without explicit locks or condition variables.



```go
func sum(s []int , c chan int)  {
   sum := 0
   for _, v := range s{
      sum += v
   }

   c <- sum

}

func main() {
   s := []int{7, 2, 8, -9, 4, 0}

   c := make(chan int)

   //分成两个切片协作计算sum
   go sum(s[:len(s)/2], c)
   go sum(s[len(s)/2:], c)

   x, y := <- c, <-c //receive from c and wait.

   fmt.Println(x, y, x+y)
}
```





### buffered channels and channel close

```go
func main() {
   channelDemo()
   bufferedchannel()
   channelclose()
}

func channelDemo() {
   var channels [10]chan<- int

   for i:=0; i<10;i++{
      channels[i] = createWorker(i)
   }

   for i:=0; i< 10 ; i++{
      channels[i] <- 'a' + i
   }


   for i:=0; i< 10 ; i++{
      channels[i] <- 'A' + i
      //x := <- channels[i] //invalid operations, receive from send only type
   }

   time.Sleep(time.Millisecond)

}

func bufferedchannel() {
   //非buffered的channel如果只发没有人收的话会报错.
   ch := make(chan int, 3)
   ch <- 1
   ch <- 2
   ch <- 3
}


//消费 with ok nil
func _worker(id int, c chan int) {
   for {
      n , ok := <- c

      if !ok{
         break
      }

      fmt.Printf("Worker %d received %d\n", id, n)
   }
}

//消费 with range
func worker(id int, c chan int) {
   //range方式, 不需要nil判断了
   for n := range c{

      fmt.Printf("Worker %d received %d\n", id, n)
   }
}

// 创建channel, 返回的channel只支持写入, 不允许取值, 即不允许消费,
// 因为消费的动作我们在createWork内部另起一个协程来做了
func createWorker(id int) chan<- int {
   c := make(chan int)
   go worker(id, c)
   return c
   
}

//channel close and range
//channel的close一定是发送方close
func channelclose()  {
   c := make(chan int)
   go worker(0, c)
   c <- 'a'
   c <- 'b'
   c <- 'c'
   close(c) //close 之后对方接受在收到3个值之后. 全部是nil值.  需要在worker里面做nil判断.
   time.Sleep(time.Millisecond)

}


```





channel 使用原则: Don't communicate by sharing memory; Share memory by communicating.



