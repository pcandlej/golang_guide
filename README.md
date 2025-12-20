# golang_guide

Go，也作 Golang，是一种过程式静态类型编程语言，其语法与 C 语言类似。它由 Google 的 Robert Griesemer、Rob Pike 和 Ken Thompson 于 2007 年开发，并于 2009 年作为开源编程语言发布，主要用于 Google 的生产系统。Golang 是开发者中最流行的编程语言之一。它提供了丰富的标准库、垃圾回收和动态类型功能。

## 概览

- [简介](overview/introduction.md)
- [Hello World!](overview/hello_world.md)

## 基本知识

- [标识符](fundamentals/identifiers.md)
- [关键词](fundamentals/keywords.md)
- [数据类型](fundamentals/data_type.md)
- [变量](fundamentals/variables.md)
- [常量](fundamentals/constants.md)
- [Rune](fundamentals/rune.md)
- [运算符](fundamentals/operators.md)
- [变量的作用域](fundamentals/scope_of_variables.md)
- [类型转换](fundamentals/type_casting.md)
- [var 关键字](fundamentals/short_variable_declaration.md)
- [短声明运算符 :=](fundamentals/short_variable_declaration.md)
- [var 关键字 vs 短声明运算符](fundamentals/var_vs_short.md)

## 函数及方法
- [可变参数函数](functions/variadic.md)

## 数组与切片

- [数组](array/array.md)
- [复制数组](array/copy_array.md)
- [函数的数组参数](array/array_as_function_parameter.md)
- [切片](array/slice.md)
- [切片复合字面量](array/slice_composite_literal.md)
- [复制切片](array/copy_slice.md)
- [切片传参](array/slice_as_function_parameter.md)
- [比较切片](array/compare_slice.md)
- [判断切片相等](array/equality_of_slice.md)
- [切片排序](array/sort_slice.md)
- [修剪字节切片](array/trimming_slice.md)
- [分割字节切片](array/splitting.md)

## 控制语句

- [判断语句](control/decision.md)
- [循环](control/loop.md)
- [循环控制](control/loop_control.md)
- [Switch 语句](control/switch.md)
- [Select 语句中的死锁和 Default Case](control/deadlock_default_case_in_select.md)

## 并发

- [协程 Goroutine](concurrency/goroutine.md)
- [Select 语句](concurrency/select.md)
- [多协程](concurrency/multi_goroutine.md)
- [Goroutine vs 线程](concurrency/goroutine_vs_thread.md)
- [Channel](concurrency/channel.md)
- [单向 Channel](concurrency/channel_direction.md)

## 接口

- [接口](interface/interface.md)
- [多接口](interface/multi_interface.md)
- [嵌入接口](interface/embedding_interface.md)
- [使用接口的多态性](interface/polymorphism.md)

## 指针

- [指针](pointer/pointer.md)
- [双重指针](pointer/double_pointer.md)
- [指针作为函数入参](pointer/pass_pointer_to_function.md)
- [指针作为函数出参](pointer/function_return_pointer.md)
- [数组的指针作为函数入参](pointer/point_to_array_as_function_parameter.md)
- [结构体的指针](pointer/point_to_struct.md)
- [比较指针](pointer/compare_pointer.md)
- [查询指针的容量](pointer/query_pointer_capacity.md)
- [查询指针的长度](pointer/query_pointer_length.md)
