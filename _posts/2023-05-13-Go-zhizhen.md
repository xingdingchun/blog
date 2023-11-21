---
layout: post
title: Go-zhizhen
key: 20230512
tags: Go
---

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# Go-zhizhen



### 关于指针



```go
变量是用来存储数据的，变量的本质是给存储数据的内存地址起了一个好记好听的名字。
指针也是一个变量，但它是一种特殊的变量，它存储的数据不是一个普通的值，而是另一个变量的内存地址。
```

![指针变量指向图](https://raw.githubusercontent.com/xingdingchun/chadwick/master/image/Go/zhizhen/zhizhen01.png)

### 指针地址和指针类型



```go
每个变量在运行时都拥有一个地址，这个地址代表变量的在内存中的位置。Go语言中使用&字符放在变量的前面对变量进行取地址操作。
Go语言中的值类型(int,float,bool,string.array,struct)都有对应的指针类型，如:*int,*int64,*string等。
```

![指针变量有自己内存](https://raw.githubusercontent.com/xingdingchun/chadwick/master/image/Go/zhizhen/zhizhen02.png)

### 输出变量a的内存地址



```go
package main

import "fmt"

func main() {
	//  在计算机底层，每一个变量都对应了一个内存地址
	var a int = 30
	fmt.Printf("a value = %v  a type = %T  a 内存地址是:%p\n", a, a, &a)

}
// 输出结果
/*
a value = 30  a type = int  a 内存地址是:0xc00001a088
*/
```



### 指针变量



```go
指针也是一个变量，但它是一个特殊的变量，它存储的数据不是一个普通的值，而是另一个变量的内存地址。
package main

import "fmt"

func main() {
	//  指针也是一个变量，但它是一个特殊的变量，它存储的数据不是一个普通的值，而是另一个变量的内存地址.
	var a int = 10
	fmt.Printf("a value = %v  a type = %T  a 内存地址是:%p\n", a, a, &a)
	p := &a  // p是指针变量,p的类型是*int指针类型
	fmt.Printf("a value = %v  a type = %T  a 内存地址是:%p\n", p, p, &p)

}
// 输出结果
/*
a value = 10  a type = int  a 内存地址是:0xc00009e058
a value = 0xc00009e058  a type = *int  a 内存地址是:0xc0000c0020
*/
```



### Go语言中所有的变量都有一个内存地址



```go
package main

import "fmt"

func main() {
	//  Go语言中所有的变量都有一个内存地址
	var a string = "chad"
	p := &a
	fmt.Printf("a的值是:%v  a的类型是:%T a的内存地址是:%p\n", a, a, &a)
	fmt.Printf("p的只是:%v  p的类型是:%T p的内存地址是:%p\n", p, p, &p)

}
// 输出结果
/*
a的值是:chad  a的类型是:string a的内存地址是:0xc000054050
p的只是:0xc000054050  p的类型是:*string p的内存地址是:0xc00000a028
注意:p是指针变量，它的值是a的内存地址，p的类型是:*string,p又有自己的内存地址
*/
```



### 使用*来取指针变量指向变量的值



```go
package main

import "fmt"

func main() {
	//  使用*来输出指针变量指向的值
	var a int = 80
	var p = &a
	fmt.Printf("a的值是:%v\n", a)
	fmt.Printf("p的只是:%v\n", p)

	b := 20
	c := &b
	fmt.Printf("c的值是:%v,c的类型是:%T,c的内存地址是:%p,c指向的内存地址变量的值是:%v\n", c, c, &c, *c)

}
// 输出结果
/*
a的值是:80
p的只是:0xc00001a088
c的值是:0xc00001a0d0,c的类型是:*int,c的内存地址是:0xc00000a030,c指向的内存地址变量的值是:20
*/
```



### 改变指针变量的值



```go
package main

import "fmt"

func main() {
	// 改变指针变量的值
	var a int = 50
	var p = &a
	fmt.Printf("a的值:%v  a的类型:%T  a的内存地址:%p\n", a, a, &p)
	fmt.Printf("p的值:%v  p的类型:%T  p的内存地址:%p  p指向的a的值是:%v\n", p, p, &p, *p)
	*p = 80
	fmt.Printf("改变p的值a的值:%v  a的类型:%T  a的内存地址:%p\n", a, a, &p)
	fmt.Printf("改变p的值p的值:%v  p的类型:%T  p的内存地址:%p  p指向的a的值是:%v\n", p, p, &p, *p)
}
// 输出结果
/*
a的值:50  a的类型:int  a的内存地址:0xc00000a028
p的值:0xc00001a088  p的类型:*int  p的内存地址:0xc00000a028  p指向的a的值是:50
改变p的值a的值:80  a的类型:int  a的内存地址:0xc00000a028
改变p的值p的值:0xc00001a088  p的类型:*int  p的内存地址:0xc00000a028  p指向的a的值是:80
*/
```



### 函数传入变量和指针变量



```go
package main

import "fmt"

// 正常传入变量
func fn01(x int) {
	x = 10

}

// 传入指针变量
func fn02(x *int) {
	*x = 40
}

func main() {
	// 函数指针变量
	var a = 50
	fn01(a)
	fmt.Printf("a的值是:%v\n", a)

	fn02(&a)
	fmt.Printf("b的值是:%v\n", a)

}
// 输出结果
/*
a的值是:50
b的值是:40
*/
```



### new()说明



```go
new函数分配内存
new是一个内置的函数，func new(Type) *Type
Type表示类型，new函数只接收一个参数，这个参数是一个类型
*Type表示类型指针，new函数返回一个指向该类型内存地址的指针
实际开发中不常用
```



### new()和make()使用



```go
package main

import "fmt"

func main() {
	// new()
	var userinfo = make(map[string]string)
	userinfo["username"] = "chad"
	userinfo["sex"] = "男"
	fmt.Println(userinfo)

	var a = make([]int, 4, 4)
	a[0] = 1
	a[1] = 2
	a[2] = 3
	a[3] = 4
	fmt.Println(a)

	// 指针也是引用数据类型
	// var b *int
	// *b = 100
	// fmt.Println(b)
	e := new(int)
	f := new(bool)
	fmt.Printf("e的值是:%v,e的类型是:%T,指针变量对应的值是:%v\n", e, e, *e)
	fmt.Printf("f的值是:%v,f的类型是:%T,指针变量对应的值是:%v\n", f, f, *f)

	var g *int
	g = new(int)
	*g = 100
	fmt.Printf("g的值是:%v\n", g)

	var r = new(bool)
	fmt.Println(r)
	fmt.Println(*r)

}
// 输出结果
/*
map[sex:男 username:chad]
[1 2 3 4]
e的值是:0xc00001a0e0,e的类型是:*int,指针变量对应的值是:0
f的值是:0xc00001a0e8,f的类型是:*bool,指针变量对应的值是:false
g的值是:0xc00001a0f0
0xc00001a0f8
false
*/
```







