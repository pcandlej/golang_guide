# Go语言int切片排序实战：3种核心方法+实用技巧
在Go语言开发中，int类型切片的排序是高频需求——无论是处理用户ID列表、统计数据排序，还是接口返回结果整理，都离不开高效的排序方案。Go标准库的`sort`包提供了简洁且性能优异的排序工具，无需手动实现复杂算法，只需灵活运用内置函数即可满足大部分场景。本文将详细拆解int切片排序的核心方法、状态检查技巧及实战注意事项，帮助开发者快速掌握排序精髓。

## 一、升序排序：`sort.Ints()`——最简洁的基础方案
`sort.Ints()`是Go语言为int切片量身打造的升序排序函数，无需自定义逻辑，一行代码即可完成排序，且性能经过底层优化，效率极高。

### 核心特性
- 原地排序：直接修改原切片，不创建新切片，内存开销小；
- 默认升序：按整数从小到大排列，适配绝大多数基础排序场景；
- 无返回值：排序结果直接作用于输入切片。

### 实战示例
```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	// 待排序的int切片
	scores := []int{85, 92, 78, 95, 63, 88}
	fmt.Println("排序前：", scores) // 输出：排序前： [85 92 78 95 63 88]

	// 调用sort.Ints()完成升序排序
	sort.Ints(scores)

	fmt.Println("排序后（升序）：", scores) // 输出：排序后（升序）： [63 78 85 88 92 95]
}
```

### 关键注意点
- 原地排序的影响：若需保留原切片数据，需先复制副本再排序（如`newSlice := append([]int{}, oldSlice...)`）；
- 支持重复元素：切片中存在重复int值时，排序后会保持相对顺序（稳定排序特性）。

## 二、排序状态检查：`sort.IntsAreSorted()`——避免无效排序
在实际开发中，有时需要先判断切片是否已排序，再决定是否执行排序操作（如批量数据处理时减少冗余计算），`sort.IntsAreSorted()`正是为此设计的工具函数。

### 核心特性
- 仅检查升序：函数仅验证切片是否按从小到大排列，不支持降序检查；
- 返回布尔值：已排序返回`true`，未排序返回`false`，逻辑直观。

### 实战示例
```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	slice1 := []int{10, 20, 30, 40}
	slice2 := []int{25, 18, 32, 14}

	// 检查切片是否已升序排序
	fmt.Println("slice1是否已排序：", sort.IntsAreSorted(slice1)) // 输出：true
	fmt.Println("slice2是否已排序：", sort.IntsAreSorted(slice2)) // 输出：false

	// 仅对未排序的切片执行排序
	if !sort.IntsAreSorted(slice2) {
		sort.Ints(slice2)
		fmt.Println("slice2排序后：", slice2) // 输出：[14 18 25 32]
	}
}
```

### 实用场景
- 数据预处理：接口返回数据可能已排序，提前检查可避免重复排序；
- 结果验证：排序后调用该函数，确保排序逻辑执行成功（尤其自定义排序场景）。

## 三、降序排序：`sort.Slice()`——灵活的自定义方案
若需按从大到小的顺序排序，`sort.Slice()`配合自定义比较函数即可实现，该方法不仅支持降序，还能扩展更多复杂排序规则（如按绝对值排序）。

### 核心特性
- 支持自定义规则：通过匿名函数定义元素比较逻辑，灵活性极高；
- 原地排序：同样直接修改原切片，性能与`sort.Ints()`相当；
- 版本要求：需Go 1.8及以上版本支持（避免旧版本编译报错）。

### 实战示例
```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	numbers := []int{45, 12, 89, 33, 67, 21}
	fmt.Println("排序前：", numbers) // 输出：排序前： [45 12 89 33 67 21]

	// 自定义比较函数：实现降序排序（a > b 则a排在前）
	sort.Slice(numbers, func(i, j int) bool {
		return numbers[i] > numbers[j]
	})

	fmt.Println("排序后（降序）：", numbers) // 输出：排序后（降序）： [89 67 45 33 21 12]
}
```

### 扩展用法（按绝对值降序）
```go
// 按整数绝对值降序排序
sort.Slice(numbers, func(i, j int) bool {
	return abs(numbers[i]) > abs(numbers[j])
})

// 辅助函数：计算绝对值
func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

## 四、实战技巧与避坑指南
### 1. 保留原切片数据
`sort.Ints()`和`sort.Slice()`均为原地排序，若需保留原数据，需先创建切片副本：
```go
original := []int{5, 3, 8, 1}
// 方法1：使用append复制
copySlice := append([]int{}, original...)
sort.Ints(copySlice)

// 方法2：使用make+copy复制
copySlice2 := make([]int, len(original))
copy(copySlice2, original)
sort.Slice(copySlice2, func(i, j int) bool {
	return copySlice2[i] > copySlice2[j]
})

fmt.Println("原切片：", original)    // 输出：[5 3 8 1]（未修改）
fmt.Println("升序副本：", copySlice) // 输出：[1 3 5 8]
fmt.Println("降序副本：", copySlice2) // 输出：[8 5 3 1]
```

### 2. 处理大型int切片
对于包含10万+元素的大型int切片，`sort`包的函数仍能保持高效——底层采用快速排序（结合插入排序优化），时间复杂度为O(n log n)，无需担心性能问题：
```go
// 生成10万个随机int的切片
largeSlice := make([]int, 100000)
for i := range largeSlice {
	largeSlice[i] = rand.Intn(1000000) // 生成0~999999的随机数
}

// 计时排序（验证性能）
start := time.Now()
sort.Ints(largeSlice)
elapsed := time.Since(start)
fmt.Printf("10万元素排序耗时：%v\n", elapsed) // 输出约1~3毫秒（视机器性能）
```

### 3. 版本兼容问题
`sort.Slice()`是Go 1.8版本引入的函数，若项目需兼容旧版本（Go 1.7及以下），可使用`sort.SliceStable()`或`sort.Sort()`配合自定义类型实现降序：
```go
// Go 1.7及以下版本兼容方案：降序排序
type IntDesc []int

// 实现sort.Interface接口的Len、Swap、Less方法
func (a IntDesc) Len() int           { return len(a) }
func (a IntDesc) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a IntDesc) Less(i, j int) bool { return a[i] > a[j] }

// 使用sort.Sort()排序
numbers := []int{45, 12, 89}
sort.Sort(IntDesc(numbers))
fmt.Println("降序排序结果：", numbers) // 输出：[89 45 12]
```

## 五、核心函数对比总结
| 函数                | 功能                          | 排序方向 | 灵活性 | 版本要求 |
|---------------------|-------------------------------|----------|--------|----------|
| `sort.Ints()`       | int切片排序                   | 升序     | 低（固定规则） | 无（全版本支持） |
| `sort.IntsAreSorted()` | 检查int切片是否升序排序       | -        | 低     | 无       |
| `sort.Slice()`      | 自定义规则排序（支持int降序） | 自定义   | 高     | Go 1.8+  |

## 六、总结
Go语言的`sort`包为int切片排序提供了“开箱即用”的解决方案：基础升序用`sort.Ints()`，简洁高效；需验证排序状态用`sort.IntsAreSorted()`，避免无效计算；降序或自定义规则用`sort.Slice()`，灵活适配复杂场景。
