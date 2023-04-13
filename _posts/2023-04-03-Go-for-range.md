---
layout: post
title: Go-for-range
key: 20230403
tags: Go
---

[欢迎访问Maihill技术博客，欢迎转载和分享](https://blog.maihill.com "Maihill技术博客")

# for...range



### for...range返回说明

Go语言中可以使用for...range遍历数组、切片、字符串、map及通道(channel)。通过for...range遍历返回值有一下规律:

1. 数组、切片、字符串返回索引和值;

2. map返回键和值;

3. 通道(channel)只返回通道内的值

### for...range遍历字符串

```go
package main

import "fmt"

func main() {
	// for...range遍历字符串
	var firstStr = "Hello World!"
	for k, v := range firstStr {
		// fmt.Println("key=", k, "value=", v)
		fmt.Printf("key=%v,value=%c\n", k, v)
	}

}
// 输出结果
/*
key=0,value=H
key=1,value=e
key=2,value=l
key=3,value=l
key=4,value=o
key=5,value=
key=6,value=W
key=7,value=o
key=8,value=r
key=9,value=l
key=10,value=d
key=11,value=!
*/
```



### for...range遍历切片



```go
package main

import "fmt"

func main() {
	// for...range遍历切片
	var arr = []string{"java", "python", "shell", "go"}
	for k, v := range arr {
		fmt.Printf("key=%v  value=%v\n", k, v)
	}

}
// 输出结果
/*
key=0  value=java
key=1  value=python
key=2  value=shell
key=3  value=go
*/
```



### for...range遍历切片



```go
package main

import "fmt"

func main() {
	// for...range遍历切片 使用fmt.Println打印输出
	var arr = []string{"java", "python", "shell", "go", "nodejs"}
	for _, v := range arr {
		fmt.Println(v)
	}

}
// 输出结果
/*
java
python
shell
go
nodejs
*/
```



### switch...case

  使用switch...case语句可方便的对大量的值进行条件判断

  Go语言规定每个switch只能有一个default分支

  基本格式

```go
  switch expression {

  case condition:

   }
```



###  练习：判断文件类型，如果后缀名是.html输出text/html,如果后缀是.css输出text/css，如果后缀是.js输出test/javascripts 使用条件判断方式实现

```go
package main

import "fmt"

func main() {
	// if条件判断

	var str = ".css"

	if str == ".html" {
		fmt.Println("text/html")
	} else if str == ".css" {
		fmt.Println("text/css")
	} else if str == ".js" {
		fmt.Println("test/javascripts")
	} else {
		fmt.Println("输入错误的文件，没有这个格式")
	}

}
// 输出结果
/*
text/css
*/
```



### 练习：判断文件类型，如果后缀名是.html输出text/html,如果后缀是.css输出text/css，如果后缀是.js输出test/javascripts 使用switch...case实现



```go
package main

import "fmt"

func main() {
	// switch...case

	var str = ".html"
	switch str {
	case ".html":
		fmt.Println("text/html")
	case ".css":
		fmt.Println("text/css")
	case ".js":
		fmt.Println("text/javascripts")
	default:
		fmt.Println("输入错误，没有这个格式")
	}

}
// 输出结果
/*
text/html
*/
```



### switch...case第二种写法



```go
package main

import "fmt"

func main() {
	// switch...case把变量设置到语句内
	switch str := ".js"; str {
	case ".html":
		fmt.Println("text/html")
	case ".css":
		fmt.Println("text/css")
	case ".js":
		fmt.Println("text/javascripts")
	default:
		fmt.Println("输入错误，此格式不存在")
	}

}
// 输入结果
/*
text/javascripts
*/
```



### 一个分支多个值判断一个数是不是偶数



```go
package main

import "fmt"

func main() {
	// switch...case一个分支上多个值

	var Fint = 8
	switch Fint {
	case 1, 3, 5, 7, 9:
		fmt.Println("不是偶数")
	case 2, 4, 6, 8, 10:
		fmt.Println("偶数")
	default:
		fmt.Println("错误")
	}

}
// 输出结果
/*
偶数
*/
```



### switch...case 输出成绩是否合格



```go
package main

import "fmt"

func main() {
	// switch...case输出成绩是否合格

	var Fstr = "E"

	switch Fstr {
	case "A", "B", "C":
		fmt.Println("合格")
	case "E":
		fmt.Println("不合格")
	default:
		fmt.Println("输入不正确")
	}

}
// 输出结果
/*
不合格
*/
```



### switch...case条件放到分支内



```go
package main

import "fmt"

func main() {
	// switch...case输出成绩是否合格条件写在分支内

	switch Fstr := "C"; Fstr {
	case "A", "B", "C":
		fmt.Println("合格")
	case "E":
		fmt.Println("不合格")
	default:
		fmt.Println("输入不正确")
	}

}
// 输出结果
/*
合格
*/
```



### switch...case分支使用表达式



```go
package main

import "fmt"

func main() {
	// 分支还可以使用表达式，这时候switch语句后面不需要在跟判断变量

	age := 61

	switch {
	case age <= 18:
		fmt.Println("快乐青少年，开开心心")
	case age < 60:
		fmt.Println("好好工作，努力创造价值")
	case age < 100:
		fmt.Println("好好享受")
	default:
		fmt.Println("输入错误")

	}

}
// 输出结果
/*
好好享受
*/
```



### switch的穿透问题fallthrought



```go
package main

import "fmt"

func main() {
	// switch的穿透问题 fallthrought
	// fallthrough语法可以执行满足条件的case的下一个case，是为了兼容c语言中的case设计的

	age := 20
	switch {
	case age <= 20:
		fmt.Println("好好学习")
		fallthrough
	case age <= 60:
		fmt.Println("好好工作")
		fallthrough
	case age < 100:
		fmt.Println("好好享受")
	default:
		fmt.Println("输入不存在的年龄")
	}

}
// 输出结果
/*
好好学习
好好工作
好好享受
*/
```

