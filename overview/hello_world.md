Hello world 是所有编程语言的第一个程序。
* golang 的文件后缀为 `.go`，字符编码 `UTF-8`，创建主程序文件：
  ```
  vim helloworld.go
  ```
* 首先在程序里添加 package main：
  ```
  package main
  ```
* 每个程序都必须以**包声明**开头。在 Go 语言中，包用于**组织**和**复用代码**。
Go 语言中有两种类型的程序：**可执行程序**和**库**。可执行程序是指*可以直接从终端运行的程序*，而库是指*可以在程序中复用的程序包*。
这里的 package main 告诉编译器，该包**必须**在可执行程序中编译，而不是在共享库中编译。它是程序的起点，并且包含 main 函数。

* 现在导入 fmt 包
  ```
  import (
    "fmt"
  )
  ```
* **import** 关键字用于在程序中导入包，**fmt** 包用于通过函数实现格式化的输入/输出。
* 在main函数中用Go语言编写打印hello world的代码：
  ```
  func main() {
    fmt.Println("Hello, World!") 
  }
  ```
* 上面的代码做了：
  * **func**: 创建一个 function
  * **main**: 它是Go语言中的main函数，不包含参数，不返回任何内容，在执行程序时调用
  * **Println()**: 此方法位于 fmt 包中，用于显示「Hello, World!」字符串。其中，Println 表示「Print line」打印一行
```golang
package main

import (
  "fmt"
)

func main() {
    fmt.Println("Hello, World!") 
}
```

**运行程序**
```bash
go run helloworld.go
```

**输出**
```
Hello, World!
```
