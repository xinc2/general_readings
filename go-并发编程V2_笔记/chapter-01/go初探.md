## 语言特性

* 开放源代码

* 静态类型和编译型

* 跨平台

* 自动垃圾回收

* 原生的并发编程

    - goroutine

    - channel

* 完善的构建工具

* 多编程范式

    - 函数为一等类型

    - 支持面向对象编程: 接口类型与实现类型

* 代码风格强制统一性

* 高效的编程和允许

* 丰富的标准库

## 安装和设置

* Go 下载portal

    - [download link](https://golang.org/dl)

* Go 源代码的相关文件

    - api/

    - bin/

    - blog/

    - doc/

    - lib/

    - misc/

    - pkg/

    - src/

    - test/

* 安装(root 权限)

    ```shell

        wget https://dl.google.com/go/go1.13.8.linux-amd64.tar.gz

        tar -C /usr/local -xzf go1.13.8.linux-amd64.tar.gz

        export GOROOT=/usr/local/go

        export PATH=$GOROOT/bin:$PATH

    ```

## 工程结构

### 工作区

* src/

* pkg/

* bin/

### GOPATH

* GOPATH中 不要包含Go语言的根目录，以便将Go语言本身的工作区和用户工作区隔离开来；

* 通过Go工具的代码获取命令 `go get`，可将指定项目的源码下载到GOPATH设定的第一个工作区，并在其中完成编译和安装

### 源码文件

  * 命令源码文件

    - main代码包属于命令源码文件，可以通过`go run`直接启动运行,通常单独放在一个代码包中,因为通常一个程序模块或者软件启动入口只有一个；

    - 库文件和命令源码文件处于同一个代码包，无法正确执行 `go build`和`go install`；

  * lib源码文件

    - 库源码文件声明的包名会与它直接所属的代码包(目录)名一致，且库文件中不应该包含无参数声明和无结果声明的`main`函数

  * 测试源码文件

    - 文件名以 `_test.go`结尾；

    - 文件中需要至少包含一个名称以`Test`开头或者`Benchmark`开头，且拥有一个类型为`*testing.T`或者`*testing.B`的参数函数；

 ### 代码包

  * 包声明

    ```go

    package "mypackage"

    ```

  * 包导入

    ```go

        import "mypackage"

    ```

    * Go 中的变量、常量、函数和类型声明可以统称为程序实体，而他们的名称统称为标识符。标识符可以使Unicode字符集中任意能表示自然语言文字的字符、数字一集下划线(_),但是不能以数字或者下划线开头。

    * 标识符首字母大小写控制着对应程序实体的访问权限。

      - 大写字母开头表示其对应的程序实体**可以**被本代码包之外的代码访到，也成为**可导出的或可公开的**；

            想要成为可导出的程序实体，需要而外满足以下两个条件:

            > 程序实体必须是非局部的。局部的程序实体是指:它被定义在 函数或结构体内部。

            > 代码包所属目录必须包含在GOPATH中定义的工作区目录

      - 小写字母开头表示其对应的程序实体**不可以**被本代码包之外的代码访到，也成为**不可导出的或包级私有的**。

    * 如果只想初始化某个包，而不需要在当前源码 文件中使用那个代码包 中的任何程序实体，可以用'_'来替代

        ```go

            import (

                _ "mypackage"

            )
        ```
  * 包初始化
    - 代码中所有全局变量的初始化会在代码包初始化init()函数执行前完成
    - 所有的代码包的init()函数都会在main()函数执行前执行完毕
    - 被导入的包中的init()函数会先执行,然后当前函数的init()函数才会执行

    ```go
        // 命令源码文件必须在这里 声明自己属于main包
        package main

        // 导入标准代码包
        import (
            "fmt"
            "runtime"
        )

        // 初始化函数
        func init(){
            // 格式化打印
            fmt.Println("Map: %v\n", m)
            info = fmt.Sprintf("OS: %s, Arch: %s", runtime.GOOS, runtime.GOARCH)
        }

        // 非局部变量，map类型且已经初始化
        var m = map[int]string{1:"A", 2:"B"}

        // 非局部变量，未初始化
        var  info string

        // main()函数,程序的入口函数
        func main(){
            fmt.Println(info)
        }

        // results
        // Map: map[1:A 2:B]
        // OS: Linux, Arch: amd64
    ```
  * 标准命令简述
    - go build: 用于编译指定的代码包或Go语言源码文件。命令源码文件会被编译成可执行文件，并存放到命令执行目录或者指定目录下。
    - go clean: 用于清除因执行其它go命令而遗留下来的临时目录和文件;
    - go doc: 用于显示go语言以及程序实体的文档；
    - go env: 用于打印Go语言相关环境;
    - go fix
    - go fmt: 用于格式化指定代码包中的Go源码文件
    - go get: 用于下载、编译并安装指定的代码包及其依赖
    - go install: 用于编译并安装指定的额代码包以及其依赖包。安装命令源码文件后，代码包所得的工作区目录的 `bin` 子目录，或者当前环境 `GOBIN` 指向的目录中会生成相应的可执行文件。安装库源码文件后，会在代码包所在的工作区目录的 `pkg` 子目录生成相应的归档文件。
    - go list: 用于显示指定代码包的信息，它可谓是代码包分析的一大便捷利器。
    - go run: 用于编译并运行 指定的命令源码文件
    - go test
    - go tool
    - go version
    - go vet

