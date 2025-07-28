# 数据类型

在 Go 语言中，数据类型分为以下四类： 
1. **基本类型**：数字、字符串和布尔值
2. **聚合类型**：数组和结构体。 
3. **引用类型**：指针、切片、映射、函数和通道
4. **接口类型**

本节只讨论**基本数据类型**，其又分为三个子类别： 
* **数字**
* **布尔值**
* **字符串**

### 数字
在 Go 语言中，数字分为三个子类别： 

* **整数**：在 Go 语言中，有符号整数和无符号整数均有四种不同的大小，如下表所示。
有符号整数用 int 表示，无符号整数用 uint 表示。 
  * 可能的算术运算：加法、减法、乘法、除法、取余数

| 数据类型 | 说明 |
| --- | --- |
|int8 |	8-bit 有符号整型
|int16 |	16-bit 有符号整型
|int32 |	32-bit 有符号整型
|int64 |	64-bit 有符号整型
|uint8 |	8-bit 无符号整型
|uint16 |	16-bit 无符号整型
|uint32 |	32-bit 无符号整型
|uint64 |	64-bit 无符号整型
|int |	int 和 uint 的大小相同，均为 32 位或 64 位
|uint |	int 和 uint 的大小相同，均为 32 位或 64 位
|rune |	是 int32 的同义词，也代表 Unicode 代码点
|byte |	是 uint8 的同义词
|uintptr |	是一个无符号整数类型。它的宽度未定义，但它可以容纳指针值的所有位

```go
// 整数的算术运算使用举例
package main

import "fmt"

func main() {

    var x int16 = 180
    var y int16 = 60
    // 加法
    fmt.Printf(" addition :  %d + %d = %d\n ", x, y, x+y)
    // 减法
    fmt.Printf("subtraction : %d - %d = %d\n", x, y, x-y)
    // 乘法
    fmt.Printf(" multiplication : %d * %d = %d\n", x, y, x*y)
    // 除法
    fmt.Printf(" division : %d / %d = %d\n", x, y, x/y)
    // 取模
    fmt.Printf(" remainder : %d %% %d = %d\n", x, y, x%y)
}
```
**输出**
```
addition :  180 + 60 = 240
subtraction : 180 - 60 = 120
multiplication : 180 * 60 = 10800
division : 180 / 60 = 3
remainder : 180 % 60 = 0
```

* **浮点数**

| 数据类型 | 说明 |
| --- | --- |
| float32 | 32-bit IEEE 754 浮点数
| float64 |	64-bit IEEE 754 浮点数

* **复数**

| 数据类型 | 说明 |
| --- | --- |
| complex64 | 包含 float32 作为实部和虚部的复数
| complex128 |	包含 float64 作为实部和虚部的复数

### 布尔值（boolean）

```go
package main
import "fmt"

func main() {

    str1 := "Hello Golang"
    str2 := "hello golang"
    str3 := "Hello Golang"
    result1:= str1 == str2
    result2:= str1 == str3
   
    fmt.Println(result1)
    fmt.Println(result2)

    fmt.Printf("The type of result1 is %T and " +
               "The type of result2 is %T", result1, result2)
   
}
```

**输出**

```
false
true
The type of result1 is bool and The type of result2 is bool
```
### 字符串

```go
package main
import "fmt"

func main() {    
   str := "Hello Golang"

   fmt.Printf("Length of the string is:%d", len(str))
   fmt.Printf("\nString is: %s", str)
   fmt.Printf("\nType of str is: %T", str)
}
```

**输出**

```
Length of the string is:12
String is: Hello Golang
Type of str is: string
```
