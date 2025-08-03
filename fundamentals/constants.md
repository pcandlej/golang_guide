# 常量

常量的值一旦被定义，就**不能**再被修改。常量可以是任何[基本类型](data_type.md)，如整数常量、浮点常量、字符常量或字符串文字。

## 定义一个常量

```go
package main

import "fmt"

const PI = 3.14

func main() 
{
    const GO_LANG = "golang"
    fmt.Println("Hello", GO_LANG)

    fmt.Println("PI is ", PI)

    const Correct = true
    fmt.Println("Have a nice day?", Correct)
}
```
**输出**
```
Hello golang
PI is  3.14
Have a nice day? true
```

## 非类型化和类型化的数字常量

类型化常量的工作方式类似于不可变变量，只能与相同类型互操作，而非类型化常量的工作方式类似于文字，
可以与相似类型互操作。在 Go 中，常量可以使用或不使用类型进行声明。
以下示例显示了有名称和无名称的类型化和非类型化数字常量。

```go
const untypedInteger  = 123
const untypedFloating = 123.45

const typedInteger       int     = 123
const typedFloatingPoint float64 = 123.45
```

## Go 语言中的常量列表如下

* 数字常量（整型常量、浮点型常量、复数常量）
* 字符串字面量(literals)
* 布尔常量

**数字常量**：数字常量是高精度值。由于 Go 是静态类型语言，不允许混合数字类型的操作。
也就是说**不能**将 float64 添加到 int，甚至**不能**将 int32 添加到 int。
但是，编写 `1e6*time.Second` 或 `math.Exp(1)` 甚至 `1<<('\t'+2.0)` 都是合法的。
常量与变量不同，其行为类似于常规数字。

**数字常量分3种**：
1. 整数
2. 浮点数
3. 复数

**整数常量**

* 前缀用于指定进制（基数）：0x 或 0X 表示十六进制，仅以 0 开头的数字表示八进制，无前缀表示十进制。
* 整数字面值还可以带有后缀，后缀由 U（大写）和 L（小写）组成，分别表示无符号和长整型。
* 它可以是十进制、八进制或十六进制常量。
* int 最多可以存储 64 位整数，有时更少。

以下是一些整数常量的示例：
```
85         /* decimal */
0213       /* octal */
0x4b       /* hexadecimal */
30         /* int */
30u        /* unsigned int */
30l        /* long */
30ul       /* unsigned long */
212         /* 合法 */
215u        /* 合法 */
0xFeeL      /* 合法 */
078         /* 非法: 8 不应出现在八进制数中 */
032UU       /* 非法: 不能重复后缀 */
```

**复数常量**

举例
```
(0.0, 0.0) (-123.456E+30, 987.654E-29)
```

**浮点型常量**

* 浮点型常量包含整数部分、小数点、小数部分和指数部分。
* 浮点型常量可以表示为十进制形式或指数形式。
* 使用十进制形式表示时，必须包含小数点、指数或两者兼有。
* 使用指数形式表示时，必须包含整数部分、小数部分或两者兼有。

以下是浮点型常数的示例
```
3.14159       /* 合法 */
314159E-5L    /* 合法 */
510E          /* 非法: 不完全指数 */
210f          /* 非法: 没有小数或指数 */
.e55          /* 非法: 缺少整数或分数 */
```

**字符串字面量**

Go语言支持两种字符串字面量：
* 双引号风格 " "
* 反引号风格 \` \`

特性说明：
* 字符串可通过 `+` 和 `+=` 运算符进行拼接
* 字符串包含三类字符（与字符字面量相同）：
    * 普通字符
    * 转义序列（如 `\n`）
    * Unicode通用字符（字符串字面量本质是无类型值）
* 字符串类型的零值为空字符串，字面量可表示为 "" 或 \`\`
* 字符串支持通过 ==、!= 等运算符进行比较

**句法**
```
// _string 结构体：表示一个字符串值
// 这是 Go 语言内部表示字符串的方式（非公开实现）
type _string struct {
    elements *byte // 指向字符串底层字节数组的指针，因此空间复杂度为O(1)
    len      int   // 字符串的字节长度（注意：不是字符数量，UTF-8 编码下可能不同）得益于此，取字符串的长度的时间复杂度为O(1)
}
```

**举例**

```go
package main

import "fmt"

func main() {
	const A = "Hello"

	var B = "World"

	var helloWorld = A + " " + B
	helloWorld += "!"

	fmt.Println(helloWorld)
	fmt.Println(A == "Hello")
	fmt.Println(B < A)
}

```

**输出**

```
Hello World!
true
false
```

**布尔常量：** 布尔常量与字符串常量类似，遵循与字符串常量相同的规则，不同之处仅在于它有两个无类型常量 true 和 false。

```go
package main

import "fmt"

func main() {
	const EVN_DEBUG = true

	type DEBUG bool

	var customDebug DEBUG = EVN_DEBUG // 既可以申明类型
	var defaultDebug = EVN_DEBUG      // 也可以不申明类型

	fmt.Println(customDebug)
	fmt.Println(defaultDebug)
}
```

const 语句可以出现在有 var 的地方，因此可以执行没有任何固定精度的算术运算。

```go
package main

import (
	"fmt"
	"math"
)

const s string = "Hello, World!"

func main() {
	fmt.Println(s)

	const n = 5
	const d = 2e10 / n

	fmt.Println(d)
	fmt.Println(int64(d))
	fmt.Println(math.Sin(n))
}
```

**输出**

```
Hello, World!
4e+09
4000000000
-0.9589242746631385
```

时间复杂度：O(1)
空间复杂度：O(1)

* 需要同时申明多个常量可以进行如下的书写方式

```go
const (
	SUCCESS = "success"
	PASS    = true
	Pi      = 3.14
)
```

