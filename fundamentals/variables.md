## 变量的命名规则

```go
package main

import "fmt"

// 包级变量命名规则（驼峰式，可导出/不可导出）
var (
    // 正确：以大写字母开头可导出
    GlobalVar int = 42

    // 正确：以小写字母开头不可导出
    localVar string = "hello"

    // 正确：包含数字但不在开头
    user2Name string = "Bob"

    // 正确：使用下划线连接（不推荐但合法）
    special_case int = 7
)

func main() {
    // === 变量命名规则示例 ===
    // 正确示例：
    var userName string  // 驼峰命名
    var dbConn string    // 缩写全大写
    var π float64 = 3.14 // Unicode字符
    var 用户  string      // 中文变量名
    var _id int          // 下划线开头（通常表示忽略）

    // 错误示例：
    // var 2faEnabled bool    // 错误：以数字开头
    // var user-name string   // 错误：包含连字符
    // var type string        // 错误：使用关键字
    // var $balance float64   // 错误：包含特殊字符

    // === 变量初始化规则 ===
    // 1. 标准初始化（显式类型）
    var age int = 30
    
    // 2. 类型推断初始化
    var name = "Alice"   // 自动推断为string
    var price = 19.99    // 自动推断为float64

    // 3. 多变量初始化
    var x, y, z int = 1, 2, 3
    var a, b, c = true, 3.14, "text"

    // 4. 短变量声明（函数内专用）
    count := 10
    email := "user@example.com"

    // 5. 零值初始化
    var salary float64  // 自动初始化为0.0
    var active bool     // 自动初始化为false
    var data []int      // 自动初始化为nil

    // 6. 表达式初始化
    var sum = x + y * z
    var greeting = "Hello, " + name

    // === 赋值规则示例 ===
    // 基本赋值
    userName = "Carol"
    
    // 多重赋值
    x, y = y, x  // 交换值
    
    // 匿名变量赋值
    _, err := fmt.Println("Hello")

    // 增量赋值
    count += 5

    // === 使用场景对比 ===
    /* 不同初始化方式适用场景：
    1. var 声明（显式类型）：
       - 需要明确指定类型时
       - 初始值与声明类型不一致时
       - 包级变量声明
    
    2. 类型推断初始化：
       - 初始值能明确表达类型时
       - 减少冗余代码
    
    3. 短变量声明（:=）：
       - 函数内部局部变量
       - 需要简洁语法时
       - 接收多返回值时
    
    4. 分组声明：
       - 声明多个相关变量时
       - 提高代码可读性
    */

    // 打印示例防止编译错误
    fmt.Println(userName, count, salary, active, sum, greeting, err)
}

/* 错误示例原因说明：
1. 以数字开头：Go标识符不能以数字开头（违反编译器规则）
2. 包含连字符：Go只允许字母/数字/下划线（特殊字符非法）
3. 使用关键字：type是Go的25个保留关键字之一
4. 特殊字符：$不是合法标识符字符（违反语言规范）
*/
```

### 命名规则
1. **合法字符**：字母（Unicode）、数字、下划线
   - 正确：`userName`, `_id`, `π`, `用户`
   - 错误：`2fa`, `user-name`, `$var`

2. **首字符限制**：
   - 必须以字母或下划线开头
   - 不能以数字开头

3. **大小写敏感**：
   - `Name` 和 `name` 是不同的变量

4. **关键字禁用**：
   - 禁止使用25个Go关键字（如var, func, if等）参考条目[关键字](fundamentals/keywords.md)

5. **命名风格**：
   - 驼峰式命名（`totalCount`）
   - 避免下划线（特殊场景除外）
   - 可导出变量必须大写开头

### 初始化规则
| 方式                | 示例                     | 适用场景                          |
|---------------------|--------------------------|----------------------------------|
| 标准初始化          | `var age int = 30`       | 需要明确指定类型                 |
| 类型推断            | `var name = "Alice"`     | 初始值能明确推断类型时           |
| 短变量声明          | `count := 10`            | 函数内部局部变量                 |
| 分组声明            | `var (x int; y string)`  | 声明多个相关变量                 |
| 零值初始化          | `var score float64`      | 变量自动初始化为类型零值         |
| 多变量初始化        | `var a, b = 1, true`     | 同时初始化多个变量               |

### 赋值规则
1. **基本赋值**：`x = 10`
2. **多重赋值**：`a, b = b, a`
3. **复合赋值**：`count += 5`
4. **匿名赋值**：`_, err = func()`
5. **强制类型要求**：
   - 类型必须匹配
   - 未初始化变量不能赋值

> **实践建议**：
> 1. 局部变量优先使用短声明（`:=`）
> 2. 包级变量使用`var`声明
> 3. 避免使用`_`开头的变量名（除非特殊需要）
> 4. 接收多返回值时使用匿名变量`_`忽略不需要的值
> 5. 未使用变量会导致编译错误（Go严格要求）
