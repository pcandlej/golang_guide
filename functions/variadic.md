# Go语言可变参数函数（Variadic Functions）
## 一、定义与核心特性
可变参数函数是Go语言中支持接收**可变数量同类型参数**的函数类型，适用于参数数量不确定的场景（如求和、批量处理数据等），其核心特性为：
- 允许调用时传递0个、1个或多个同类型参数；
- 函数内部会将可变参数视为**切片（slice）** 处理，可通过遍历切片获取所有参数。

## 二、语法规则
### 1. 定义语法
```go
func 函数名(参数名 ...参数类型) 返回类型 {
    // 函数体（可变参数可作为切片访问）
}
```
- 关键标识：参数类型后加 ellipsis（`...`），表示该参数为可变参数；
- 示例：`func sum(nums ...int) int` 定义了接收可变数量int型参数的求和函数。

### 2. 调用语法
- 直接传递多个同类型参数：`sum(1, 2, 3)`；
- 传递0个参数（合法）：`sum()`；
- 函数内部通过切片遍历处理参数：`for _, n := range nums { ... }`。

## 三、混合常规参数的使用规则
可变参数可与常规参数搭配使用，但需遵循严格限制：
- **可变参数必须是函数的最后一个参数**；
- 同一函数中只能有一个常规参数，其余为可变参数。

### 示例
```go
// prefix为常规参数，nums为可变参数（最后一个）
func greet(prefix string, nums ...int) {
    fmt.Println(prefix)
    for _, n := range nums {
        fmt.Println("Number:", n)
    }
}
// 合法调用
greet("Sum of numbers:", 1, 2, 3)
greet("No numbers sum:") // 可变参数传递0个值
```

## 四、核心限制
1. 同一函数**只能定义一个可变参数**；
2. 可变参数**必须位于参数列表的末尾**，不能在常规参数之前；
3. 可变参数的所有传入值必须是**同一类型**（与定义的参数类型一致）。

## 五、典型示例与输出
### 求和函数示例
```go
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}

// 调用与输出
fmt.Println(sum(1,2,3)) // 输出：6
fmt.Println(sum(4,5))   // 输出：9
fmt.Println(sum())      // 输出：0（0个参数时返回初始值0）
```

### 混合参数示例输出
```
Sum of numbers:
Number: 1
Number: 2
Number: 3
Another sum:
Number: 4
Number: 5
No numbers sum: // 0个可变参数时仅输出常规参数内容
```
