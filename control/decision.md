# 判断语句

* Go 编程的判断语句是：
  * **if**
  * **if-else**
  * **嵌套if**
  * **if-else-if**

> 与多数语言的不同，Go 判断语句的条件部分**不会**写在括号中

```go
package main

import "fmt"

func main() {
    // 定义基础变量：分数和是否为补考
    score := 75
    isRetake := false

    // 1. 基础if语句：判断分数是否达标
    if score >= 60 {
        fmt.Println("恭喜，成绩合格！")
    }

    // 2. if-else语句：判断是否需要补考
    if isRetake {
        fmt.Println("本次为补考成绩")
    } else {
        fmt.Println("本次为正常考试成绩")
    }

    // 3. 嵌套if语句：在合格基础上判断是否优秀
    if score >= 60 {
        // 嵌套内层if判断高分段
        if score >= 90 {
            fmt.Println("成绩优秀，获得A等级")
        }
    }

    // 4. if-else-if阶梯：多条件评级
    if score >= 90 {
        fmt.Println("评级：A（优秀）")
    } else if score >= 80 {
        fmt.Println("评级：B（良好）")
    } else if score >= 60 {
        fmt.Println("评级：C（及格）")
    } else {
        fmt.Println("评级：D（不及格）")
    }

    // 演示修改变量后的值判断
    score = 55
    if score < 60 {
        fmt.Printf("当前分数%d，需要参加补考\n", score)
    }
}
```
