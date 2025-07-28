# 标识符

标识符是程序组件的用户定义名称。在 Go 语言中，标识符可以是**变量名**、**函数名**、**常量**、**语句标签**、**包名**或**类型**。

**举例**：

```golang
package main
import "fmt"

func main() {

 var name = "hello golang"
  
}
```
上面的例子中一共有三个标识符： 
* **main**：包的名称
* **main**：函数的名称
* **name**：变量的名称

**定义标识符的规则**：定义有效的 Go 标识符有一些有效的规则。必须遵循这些规则，否则将引发编译时错误。
* 标识符名称必须以**字母**或**下划线 (_) **开头。
* 名称可以包含字母“a-z”或“A-Z”或数字 0-9，也可以包含字符“_”。
* 标识符名称**不能**以**数字开头**。标识符名称**区分大小写**。
* **不允许**使用**关键字**作为标识符名称。
* 标识符名称的长度没有限制，但建议长度保持在 4 到 15 个字母之间。

* 有一些预声明的标识符可用于常量、类型和函数。这些名称不是保留的，你可以在声明中使用它们。以下是预声明标识符的列表：
  ```
  常量:
  true, false, iota, nil
  
  类型:
  int, int8, int16, int32, int64, uint,
  uint8, uint16, uint32, uint64, uintptr,
  float32, float64, complex128, complex64,
  bool, byte, rune, string, error
  
  方法:
  make, len, cap, new, append, copy, close, 
  delete, complex, real, imag, panic, recover
  ```
  * [预声明标识符为什么不是保留的？一般会有人用预声明标识符作为变量常量函数来使用吗？](identifiers_ex.md)
* 下划线字符 (_) 称为空白标识符。它用作**匿名占位符**，而不是常规标识符，在声明、操作数和赋值中具有特殊含义。
* 允许从其他包访问的标识符称为**导出标识符**。导出标识符满足以下条件：
  * 导出的标识符名称的第一个字符应为 Unicode 大写字母。
  * 标识符应该在包块中声明，或者是该包内的变量、函数、类型或方法名称。
* 在下面的示例中，file1.go 包含一个名为 ExportedVariable 的导出变量，该变量可在同一文件中访问。它还导入了 file2 包，并从 file2.go 访问导出变量 AnotherExportedVariable。运行 go run file1.go 后，它将打印 file1.go 中 ExportedVariable 的值（“Hello, World!”）和 file2.go 中 AnotherExportedVariable 的值（“Greetings from file2!”）。这演示了 Go 中导出标识符可从其他包访问的概念。

**file2.go**

  ```golang
  package file2
  
  // 导出变量
  var AnotherExportedVariable = "Greetings from file2!"
  ```

**file1.go**

  ```golang
  // file1.go
  
  package main
  
  import (
      "fmt"
      "github.com/yourusername/project/file2"
  )
  
  // 导出变量
  var ExportedVariable = "Hello, World!"
  
  func main() {
      // Accessing exported identifier in the same file
      fmt.Println(ExportedVariable)
  
      // Accessing exported identifier from another package
      fmt.Println(file2.AnotherExportedVariable)
  }
  ```

**输出**

```
Hello, World!
Greetings from file2!
```

* **标识符的唯一性**
  * 在当前作用域（如一个文件/包）中，每个标识符的名字**必须**独一无二
  * 同时，首字母小写的标识符**不会**暴露给**外部包**

  >
  > ❗注意：相同包（package）内的文件是**可以**访问各自的私有标识符的
  > 
  >   所有同包文件共享同一个包作用域，编译器将同包所有文件合并处理
  > 
  >   因此相同包内也**不能**定义重复的私有标识符
  >

```
package main

import "fmt"

// ✅ 导出标识符：首字母大写（外部包可访问）
func PublicFunc() {
    fmt.Println("我是导出的函数")
}

// ❗ 私有标识符：首字母小写（仅本包可用）
func privateFunc() {
    fmt.Println("我是私有函数")
}

var globalVar int // 私有变量（首字母小写），本包唯一，仅本包程序可以访问

func main() {
    var localVar int  // ✅ 局部变量（仅在main函数内唯一）
    fmt.Println(localVar)
}
```



  
