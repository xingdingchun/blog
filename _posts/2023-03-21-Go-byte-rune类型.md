---
layout: post
title: Go-byte-rune类型
key: 20230321
tags: Go
---

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# Go-byte-rune类型





### 定义一个byte类型



> 组成每个string的元素叫做“字符”，可以遍历string元素来获取字符，字符用单引号('')来定义；
>
> 当直接输出byte字符类型的时候输出的结果是字符对应的码值(字母符号对应的是ASCII)；



```go
package main

import (
	"fmt"
)

func main() {
	// 组成每个字符串string的元素叫做 "字符" ，可以遍历字符串元素获得字符，字符用单引号(')包裹起来
	// 定义一个字符(byte)类型
	var byteA = 'a'
	var byteB = '2'
	fmt.Printf("byteA的值是:%v byteA的类型是:%T\nbyteB的值是:%v byteB的类型是:%T", byteA, byteA, byteB, byteB)

}
// 输出结果
/*
byteA的值是:97 byteA的类型是:int32
byteB的值是:50 byteB的类型是:int32
*/
```



### 原样输出byte类型



```go
package main

import (
	"fmt"
)

func main() {
	// 原样输出byte类型
	var byteA = 'c'
	var byteB = '8'
	fmt.Printf("byteA的值是:%c byteA的类型是:%T\nbyteB的值是:%c byteB的类型是:%T", byteA, byteA, byteB, byteB)

}
// 输出结果
/*
byteA的值是:c byteA的类型是:int32
byteB的值是:8 byteB的类型是:int32
*/
```



### Go语言中字符的种类



> 1. uint8类型或者叫byte型，代表ASCII码表的一个字符；
> 2. rune类型，代表UTF-8码表的一个字符;



### 定义一个字符串输出字符串里面的字符



```go
package main

import (
	"fmt"
)

func main() {
	// 定义一个字符串输出字符串里面的字符
	var stringA = "beautiful"
	fmt.Printf("stringA下标0的源值是:%c stringA下标0的值:%v stringA下标0的类型是:%T\n", stringA[0], stringA[0], stringA[0])
	fmt.Printf("stringA下标2的源值是:%c stringA下标2的值:%v stringA下标2的类型是:%T\n", stringA[2], stringA[2], stringA[2])
	fmt.Printf("stringA下标8的源值是:%c stringA下标8的值:%v stringA下标8的类型是:%T\n", stringA[8], stringA[8], stringA[8])
}
// 输出结果
/*
stringA下标0的源值是:b stringA下标0的值:98 stringA下标0的类型是:uint8
stringA下标2的源值是:a stringA下标2的值:97 stringA下标2的类型是:uint8 
stringA下标8的源值是:l stringA下标8的值:108 stringA下标8的类型是:uint8
*/
```



### 字符和汉字占用空间



```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	// 字符和汉字占用空间
	var stringA = "1"
	var stringB = "美好"
	var intA int16 = 10

	fmt.Println("stringA占用的空间", len(stringA))
	fmt.Println("stringB占用的空间", len(stringB))
	fmt.Println("intA占用的空间", unsafe.Sizeof(intA))

}
// 输出结果
/*
stringA占用的空间 1
stringB占用的空间 6
intA占用的空间 2
*/
```



### 定义汉字字符



```go
package main

import (
	"fmt"
)

func main() {
	// 定义一个字符，字符的值是汉字
	// go中汉字使用的是UTF-8编码，编码后的值是int类型,Unicode编码是10进制
	var byteA = '美'

	fmt.Printf("byteA的值:%v,byteA的类型:%T\n", byteA, byteA)
	fmt.Printf("byteA的原值:%c,byteA的类型:%T\n", byteA, byteA)

}
// 输出结果
/*
byteA的值:32654,byteA的类型:int32
byteA的原值:美,byteA的类型:int32
*/
```



### 循环输出字符



```go
// for range循环 相当于rune类型
package main

import "fmt"

func main() {
	// 通过循环输出string中的字符
	var stringA = "beautiful美好"
	for k, v := range stringA {
		fmt.Printf("stringA的下标:%v stringA的值:%v stringA的原值:%c stringA的类型:%T\n", k, v, v, v)
	}

}
// 输出结果
/*
stringA的下标:0 stringA的值:98 stringA的原值:b stringA的类型:int32
stringA的下标:1 stringA的值:101 stringA的原值:e stringA的类型:int32    
stringA的下标:2 stringA的值:97 stringA的原值:a stringA的类型:int32     
stringA的下标:3 stringA的值:117 stringA的原值:u stringA的类型:int32    
stringA的下标:4 stringA的值:116 stringA的原值:t stringA的类型:int32    
stringA的下标:5 stringA的值:105 stringA的原值:i stringA的类型:int32    
stringA的下标:6 stringA的值:102 stringA的原值:f stringA的类型:int32    
stringA的下标:7 stringA的值:117 stringA的原值:u stringA的类型:int32    
stringA的下标:8 stringA的值:108 stringA的原值:l stringA的类型:int32    
stringA的下标:9 stringA的值:32654 stringA的原值:美 stringA的类型:int32 
stringA的下标:12 stringA的值:22909 stringA的原值:好 stringA的类型:int32
*/

// 使用for循环的方式 相当于byte类型
package main

import "fmt"

func main() {
	// 通过循环输出string中的字符
	var stringA = "beautiful美好"
	for i := 0; i < len(stringA); i++ {
		fmt.Printf("stringA的下标:%v stringA的值:%v stringA的原值:%c stringA的类型:%T\n", i, stringA[i], stringA[i], stringA[i])
	}

}
// 输出结果
/*
stringA的下标:0 stringA的值:98 stringA的原值:b stringA的类型:uint8
stringA的下标:1 stringA的值:101 stringA的原值:e stringA的类型:uint8 
stringA的下标:2 stringA的值:97 stringA的原值:a stringA的类型:uint8  
stringA的下标:3 stringA的值:117 stringA的原值:u stringA的类型:uint8 
stringA的下标:4 stringA的值:116 stringA的原值:t stringA的类型:uint8 
stringA的下标:5 stringA的值:105 stringA的原值:i stringA的类型:uint8 
stringA的下标:6 stringA的值:102 stringA的原值:f stringA的类型:uint8 
stringA的下标:7 stringA的值:117 stringA的原值:u stringA的类型:uint8 
stringA的下标:8 stringA的值:108 stringA的原值:l stringA的类型:uint8 
stringA的下标:9 stringA的值:231 stringA的原值:ç stringA的类型:uint8 
stringA的下标:10 stringA的值:190 stringA的原值:¾ stringA的类型:uint8
stringA的下标:11 stringA的值:142 stringA的原值: stringA的类型:uint8
stringA的下标:12 stringA的值:229 stringA的原值:å stringA的类型:uint8
stringA的下标:13 stringA的值:165 stringA的原值:¥ stringA的类型:uint8
stringA的下标:14 stringA的值:189 stringA的原值:½ stringA的类型:uint8
*/
```



### 通过字符修改string



```go
// 1 uint8类型或者叫byte型，代表了ascii码的一个字符
// 2 rune类型，代表一个UTF-8字符
package main

import "fmt"

func main() {
	// 通过字符修改字符串
	var stringA = "beautiful"
	var byteA = []byte(stringA)
	byteA[0] = 'B'
	fmt.Printf("输出修改后的stringA的值:%v\n", string(byteA))

	var stringB = "美好"
	var byteB = []rune(stringB)
	byteB[1] = '丽'
	fmt.Printf("输出修改后的stringB的值:%v\n", string(byteB))

}
// 输出结果
/*
输出修改后的stringA的值:Beautiful
输出修改后的stringB的值:美丽 
*/
```
