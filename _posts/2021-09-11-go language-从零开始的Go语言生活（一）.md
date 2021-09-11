---
layout: post
title: "从零开始的Go语言生活（一）"
date: 2021-09-11
excerpt: "GO语言入门第一节“
tags: [Go语言，入门，基础]
comments: true
---


​	最近在找工作屡屡碰壁，面试官给的意见都是我会的东西虽然多但是没有专精......于是在咨询了小爱姐姐以及大咩等一众大佬后决定开始Go语言的学习，早日成为996打工人（不是）。以此博客来纪念我的从零开始的Go语言学习生活！

## 1.安装

​	工欲善其事，必先利其器。首先按照官网（https://golang.google.cn/doc/install）的安装步骤将Go安装到电脑上，这一步没什么问题，无脑下一步就好了。

​	代码编辑器使用Vs code，下载安装好后安装Go语言的插件。可能会遇到网络问题无法安装，可选择使用魔法或者离线包安装。我采用的是第二种方式，步骤如下：

1.首先在官网 https://marketplace.visualstudio.com/VSCode  搜索需要安装的插件，下载。

2.打开Vs code ，在左侧的最底部选择extension，在弹出框中选择视图和更多操作->从vslx安装。

3.提示重启Vs code，按提示重启即可。

## 2.“Hello world！”

​	在安装完所需的软件后，来到了学习编程的第一步：“Hello world！"。

​	1.打开命令行，cd到项目工程文件夹，创建文件夹Hello，cd Hello。

​	2.然后运行以下命令：

```shell
	go mod init example/hello
```

​	3.接着在vs code里创建Hello.go，并输入以下代码：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}

```

​	4.在命令行中输入

```shell
go run .
Hello, World!
```

​	显示成功，完结撒花！

## 3.刚刚发生了什么

​	回顾刚刚的步骤，将重点放在第二步和第三步中。

```shell
	go mod init example/hello
```

​	在第二步运行了上方的命令，对整个Hello模块进行了初始化，在Hello文件夹下创建了名为Hello.mod的模块配置文件，模块配置文件用于说明模块运行需要什么样的包。example/hello是指的此模块的具体位置，如想在Github中初始化这样一个模块，需将上述的example/hello替换为你Github存放此模块的地址。

​	

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}

```

​	若想执行Go语言，必须要有一个主函数入口，在上述代码中使用func main()定义了主函数入口；和其它语言一样，Go语言也提供了标准的输入输出函数，fmt为标准的输出函数用于提供格式化输出以及将输出输出至控制台的函数。在运行go run . 后，go会执行主函数，将Hello world输出至控制台。

## 4.引用其它模块

​	接下来官方教我们如何引用其它的包。官方提供了一个用于搜索可用包的网址（https://pkg.go.dev/），在此网址搜索quote，找到rsc.io/quote，点击进去可以看到其中可用的函数。

​	再在我们的Hello.go中将代码修改成：

```go
package main

import "fmt"

import "rsc.io/quote"

func main() {
    fmt.Println(quote.Go())
}
```

​	后使用如下命令载入所引的包。

```shell
go mod tidy
```

​	这一步如果是在国内没有魔法的情况下会发生错误，设置国内代理即可。命令为：

```shell
go env -w GOPROXY=https://goproxy.cn
```

​	在成功导入包后运行即可，成功撒花❀。

## 5.使用自己创建的模块

​	官方接下来教我们如何创建自己的模块。

​	在之前所建立的hello文件夹的同级目录下，建立greeting目录，进入此目录后使用以下命令初始化greeting包：

```shell
go mod init example.com/greetings
```

​	新建greeting.go，写入以下代码：

```go
package greetings

import "fmt"

// Hello returns a greeting for the named person.
func Hello(name string) string {
    // Return a greeting that embeds the name in a message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message
}
```

​	至此完成了包的构建。

​	将重点放在第二段代码中：首先第一行声明了包名，意味着在此声明下的所有函数都属于此包；引入fmt包后定义了Hello函数，此函数接收一个名为name的String参数，返回值亦为String，在声明参数传递时变量名在前变量类型在后（有点不习惯）；“message：=”等同于代码"var message，message="（这种通过自动变量的形式判断变量类型不知道会不会像Python或者C++一样会降低运行的速度，问题先放在这，调查后再看）。

​	接下来调用我们刚生成的包，回到前面创建的hello文件夹，将代码修改如下：

```Go
package main

import (
    "fmt"

    "example.com/greetings"
)

func main() {
    // Get a greeting message and print it.
    message := greetings.Hello("Gladys")
    fmt.Println(message)
}
```

​	在此处引入了刚刚创建的包，并调用其中的Hello函数，打印获取的结果。但此时直接使用go mod tidy会报错，因为com.example实际并不存在，需要将路径改为greeting包实际存在的路径。使用以下命令完成：

```shell
 go mod edit -replace example.com/greetings=../greetings
```

​	或者直接在mod文件末尾添加：

```
replace example.com/greetings => ../greetings
```

​	以上两种方式均可，完成后运行：

```shell
go mod tidy
```

​	go会自动在文件末尾添加一行：

```
require example.com/greetings v0.0.0-00010101000000-000000000000
```

​	由于并没有在greeting中的 go.mod 文件声明版本号，go自动生成了这么一串数字。关于版本的详细信息另外再开一贴，不做赘述。

​	运行go run .，完结撒花。

## 6.总结

​	首先是go的常用的命令行：

```shell
go mod init 包名 //初始化模块
go mod tidy //根据引入的包从指定路径加载对应的模块
go env -w GOPROXY=https://goproxy.cn //修改go的代理
go run . //运行此目录下的主程序
```

​	go语言的一些规定：

​	函数定义：

```go
func function_name(var_name var_type) function_type{
//code 
}
```

​	变量定义：

```Go
var variance (type) //def a variance
varity:=value //def and init a variance
```

​	声明包：

```go
package name
```

​	引包:

```go
import (
	"location.of/package"
	"anotherpackage"
)
```

​		自此结束，以上，撒花❀。