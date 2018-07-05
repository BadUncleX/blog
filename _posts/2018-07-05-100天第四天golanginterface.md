---
layout: post
title:  " 100天第四天 golang methods interface "
date:   2018-07-04 08:51:31 +0800
categories: golang 100days
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}


## add methods for type



```go
type Vertexs struct {
	X, Y float64
}

func (v Vertexs)Abs() float64 {
	return math.Sqrt(v.X * v.X + v.Y*v.Y)

}

func Abs(v Vertexs) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertexs{3, 4}
    //两种方式, 同样效果
	fmt.Println(v.Abs())
	fmt.Println(Abs(v))
}
```



## Pointer receivers

```go
type Vertexs struct {
   X, Y float64
}

func (v Vertexs)Abs() float64 {
   return math.Sqrt(v.X * v.X + v.Y*v.Y)

}

func Abs(v Vertexs) float64 {
   return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
   v := Vertexs{3, 4}
   v.Scale(10)
   fmt.Println(v.Abs())
}

//可以修改receiver
//在需要修改原始传入的值的情况才用pointer
func (v *Vertexs)Scale(f float64)  {
   v.X = v.X * f
   v.Y = v.Y * f
}
```



## pointer and functions

上面的pointer作为receiver简洁, 如果是普通方法参数用pointer需要&引用

```go
type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func Scale(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	Scale(&v, 10) //&v 指针变量
	fmt.Println(Abs(v))
}
```



## 普通function方式参数严格

```go
var v Vertex
ScaleFunc(v, 5)  // Compile error!
ScaleFunc(&v, 5) // OK
```



```go
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```



That is, as a convenience, Go interprets the statement `v.Scale(5)` as `(&v).Scale(5)` since the `Scale` method has a pointer receiver.

> type的调用方式会有间接转换





## Choosing a value or pointer receiver

There are two reasons to use a pointer receiver.

The first is so that the method can **modify the value** that its receiver points to.

The second is to **avoid copying the value** on each method call. This can be more efficient if the receiver is a large struct.



```go
type Vertex struct {
	X, Y float64
}

//可以修改值
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

//避免值拷贝
func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	fmt.Printf("Before scaling: %+v, Abs: %v\n", v, v.Abs())
	v.Scale(5)
	fmt.Printf("After scaling: %+v, Abs: %v\n", v, v.Abs())
}
```



## interface接口实现



```go
type I interface {
	M()
}

type T struct {
	S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}


```

Interfaces are implemented **implicitly**
A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "**implements**" keyword.

> interface没有显式implements关键词, 是用type作为receiver隠式实现方法. 



## The empty interface

The interface type that specifies zero methods is known as the *empty interface*:

 

```go
func do(i interface{}) {//empty interface, holds any type 
	switch v := i.(type) {//get interface type
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}

func main() {
	do(21)
	do("hello")
	do(true)
}
```



## Stringers

> 类似java的toString()



```go
type Person struct {
   Name string
   Age int
}

func (p Person) String() string {
   return fmt.Sprintf("%v (%v years)\n", p.Name, p.Age)
}

func main() {
   a := Person{"Arthur Dent", 42}
   z := Person{"Zaphod Beeblebrox", 9001}

   //隠式调用a.String(), z.String()
   fmt.Println(a, z)
}
```



example: ipaddress parse, array to string

```go
func (ip IPAddr) String() string  {
   res := fmt.Sprintf("%v",ip[0] )

   for _, b := range ip[1:]{
      res += fmt.Sprintf(".%v", b)

   }
   return res
}

func ipHost() {
   host := map[string]IPAddr{
      "loopback":  {127, 0, 0, 1},
      "googleDNS": {8, 8, 8, 8},
   }

   for name, ip := range host{

      fmt.Printf("%v: %v \n", name, ip)

      //get variable type
      fmt.Println(reflect.TypeOf(ip))
   }
}
```

