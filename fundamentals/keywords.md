## Go 共 25 个关键字

### 1. 程序结构
| 关键字   | 示例                          | 作用                     |
|----------|-------------------------------|--------------------------|
| **package** | `package main`                | 声明当前文件所属包       |
| **import** | `import "fmt"`                | 导入外部包               |
| **func**   | `func main() {}`              | 声明函数                 |
| **return** | `return 42`                   | 从函数返回值             |

---

### 2. 变量与类型
| 关键字   | 示例                          | 作用                     |
|----------|-------------------------------|--------------------------|
| **var**    | `var x int`                   | 声明变量                 |
| **const**  | `const Pi = 3.14`             | 声明常量                 |
| **type**   | `type Point struct{ X,Y int }`| 声明自定义类型           |
| **struct** | `type User struct{ Name string }` | 定义结构体类型       |
| **interface** | `type Writer interface{ Write() }` | 定义接口类型       |
| **map**    | `m := make(map[string]int)`   | 创建键值对集合           |
| **chan**   | `ch := make(chan int)`        | 创建通信通道             |

---

### 3. 流程控制
| 关键字     | 示例                          | 作用                     |
|------------|-------------------------------|--------------------------|
| **if**       | `if x > 0 { ... }`            | 条件判断                 |
| **else**     | `if x>0{...} else{...}`       | 条件分支                 |
| **for**      | `for i:=0; i<10; i++{...}`    | 循环控制                 |
| **range**    | `for k,v := range m {...}`    | 迭代集合元素             |
| **switch**   | `switch x { case 1: ... }`    | 多条件选择               |
| **case**     | `case "apple": ...`           | 分支条件                 |
| **default**  | `default: ...`                | 默认分支                 |
| **break**    | `break`                       | 跳出循环/switch          |
| **continue** | `continue`                    | 跳过当前循环迭代         |
| **goto**     | `goto Cleanup`                | 无条件跳转               |
| **fallthrough** | `fallthrough`             | 强制执行下一case         |

---

### 4. 并发控制
| 关键字   | 示例                          | 作用                     |
|----------|-------------------------------|--------------------------|
| **go**     | `go process()`                | 启动新goroutine          |
| **select** | `select { case <-ch: ... }`   | 多通道操作选择器         |

---

### 5. 延迟与错误处理
| 关键字   | 示例                          | 作用                     |
|----------|-------------------------------|--------------------------|
| **defer**  | `defer file.Close()`          | 延迟执行（栈式）         |

---

### 示例集合
```go
package main

import "fmt"

// 类型与变量
type Point struct{ X, Y int }
const Pi = 3.14
var global = 10

// 接口与通道
type Writer interface{ Write([]byte) }
var ch = make(chan int)

func main() {
    // 流程控制
    if x := 5; x > 0 {
        for i := 0; i < 3; i++ {
            switch {
            case i == 1:
                fmt.Print("one ")
                fallthrough
            default:
                fmt.Print("default ")
            }
        }
    }
    
    // 并发
    go func() { ch <- 1 }()
    select {
    case <-ch:
        fmt.Print("received ")
    }
    
    // 延迟
    defer fmt.Print("last")
    
    // 跳转
    goto End
    fmt.Print("skipped")
End:
    fmt.Println("end")
}
// 输出: one default default received last end
```

> **关键设计哲学**：  
> Go 的关键字设计体现 **"极简主义"**：  
> 1. 仅 25 个关键字（对比：C++ 84个, Java 53个）  
> 2. 无 `class`/`public`/`private` 等传统OOP关键字  
> 3. 用 `chan`/`go` 直接支持并发取代繁琐的线程库  
> 4. `defer` 简化资源清理，避免嵌套 try-finally
