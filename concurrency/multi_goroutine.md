# Go语言多协程进阶：WaitGroup与Worker Pool模式实战

在Go语言的并发编程中，基础的`go`关键字只是起点。当需要管理多个协程的生命周期、协调任务分工时，`sync.WaitGroup`和`Worker Pool（工作池）模式`能让代码更健壮、可控。本文将深入探讨这两种进阶用法。


## 一、sync.WaitGroup：优雅管理协程生命周期

基础示例中使用`time.Sleep`等待子协程完成的方式并不可靠（无法精确预估执行时间）。`sync.WaitGroup`提供了更科学的解决方案，它通过"计数等待"机制，确保主协程在所有子协程完成后再退出。

### 核心方法
- `Add(n int)`：添加需要等待的协程数量（n为正整数）
- `Done()`：协程完成后调用，相当于将计数减1
- `Wait()`：主协程阻塞等待，直到计数归0

### 实战示例：用WaitGroup替代Sleep
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

// 带WaitGroup参数的任务函数
func printNumbers(wg *sync.WaitGroup, id int) {
    defer wg.Done() // 函数退出时自动调用Done()，确保计数减1
    for i := 0; i < 3; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Printf("协程%d: %d\n", id, i)
    }
}

func main() {
    var wg sync.WaitGroup
    wg.Add(2) // 声明需要等待2个协程

    fmt.Println("主协程启动")
    go printNumbers(&wg, 1) // 启动协程1
    go printNumbers(&wg, 2) // 启动协程2

    wg.Wait() // 阻塞等待所有协程完成
    fmt.Println("主协程结束")
}
```

**输出结果**：
```
主协程启动
协程1: 0
协程2: 0
协程1: 1
协程2: 1
协程1: 2
协程2: 2
主协程结束
```

**优势**：无需硬编码等待时间，无论子协程执行多久，主协程都会精确等待所有任务完成。


## 二、Worker Pool模式：高效分配并发任务

当需要处理大量任务时，无限制创建协程可能导致资源耗尽。Worker Pool模式通过预先创建固定数量的"工作协程"，从任务队列中获取任务执行，实现资源控制与任务分发的平衡。

### 核心组件
- **任务队列**：通常用channel传递任务数据
- **工作协程（Worker）**：固定数量的协程，循环从队列中获取任务并执行
- **任务提交器**：向队列中发送任务的组件
- **退出机制**：通过关闭channel或发送特殊信号通知Worker退出

### 实战示例：批量处理数据的Worker Pool
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

// 定义任务类型（例如需要处理的数据）
type Task struct {
    ID     int
    Data   string
    Result string
}

// 工作协程：循环处理任务
func worker(id int, tasks <-chan Task, results chan<- Task, wg *sync.WaitGroup) {
    defer wg.Done()
    for task := range tasks { // 从任务channel接收任务，channel关闭后退出循环
        fmt.Printf("工作协程%d 处理任务%d: %s\n", id, task.ID, task.Data)
        time.Sleep(500 * time.Millisecond) // 模拟处理耗时
        task.Result = "处理完成：" + task.Data
        results <- task // 将结果发送到结果channel
    }
}

func main() {
    const (
        workerCount = 3    // 工作协程数量
        taskCount   = 8    // 总任务数
    )

    // 创建任务channel和结果channel
    tasks := make(chan Task, taskCount)
    results := make(chan Task, taskCount)

    var wg sync.WaitGroup

    // 启动工作协程
    wg.Add(workerCount)
    for i := 1; i <= workerCount; i++ {
        go worker(i, tasks, results, &wg)
    }

    // 提交任务
    go func() {
        for i := 1; i <= taskCount; i++ {
            tasks <- Task{ID: i, Data: fmt.Sprintf("原始数据%d", i)}
        }
        close(tasks) // 任务提交完成，关闭channel（通知Worker退出）
    }()

    // 等待所有Worker完成并关闭结果channel
    go func() {
        wg.Wait()
        close(results)
    }()

    // 收集结果
    for res := range results {
        fmt.Printf("任务%d 结果: %s\n", res.ID, res.Result)
    }

    fmt.Println("所有任务处理完毕")
}
```

**执行流程解析**：
1. 初始化3个工作协程和8个任务
2. 任务提交器向`tasks`channel发送所有任务后关闭channel
3. 每个工作协程从`tasks`中取任务处理，处理完的结果发送到`results`
4. 主协程从`results`中收集所有结果，直到channel关闭

**优势**：
- 控制并发数量（3个Worker），避免资源滥用
- 任务与结果通过channel传递，天然实现同步
- 可扩展性强，通过调整Worker数量适应不同负载


## 三、多协程编程最佳实践

1. **避免裸协程**：始终用`WaitGroup`或`channel`管理协程生命周期，防止主协程退出导致子协程被强制终止。
2. **限制协程数量**：高并发场景下使用Worker Pool，根据CPU核心数或业务需求设置合理的Worker数量（通常为`runtime.NumCPU()`的1-4倍）。
3. **明确资源边界**：多个协程操作共享资源时，需用`sync.Mutex`（互斥锁）或channel实现同步，避免数据竞争。
4. **优雅退出**：通过`关闭channel`或使用`context.Context`传递取消信号，让协程有机会释放资源后再退出。

通过`sync.WaitGroup`和Worker Pool模式，能有效解决多协程管理中的常见问题，让Go语言的并发能力在实际开发中发挥更大价值。这些模式广泛应用于Web服务器、数据处理引擎等场景，是Go开发者必须掌握的核心技能。