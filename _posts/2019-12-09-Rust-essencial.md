---
layout: post
title:  " Rust 语言精要 "
date:   2019-12-09 04:51:28 +0800
categories: rust
math: true
typora-copy-images-to: ../assets/images/2019-12
typora-root-url: ../../blog_alexcode
---
> 编程之道之语言精要章节笔记


* TOC
{:toc}




## Rust 语言的基本构成

- 语言规范
  - Rust Reference（语言结构用法描述、对内存模型、并发模型等内存描述， 基本原理和参考）
  - RFC (涵盖语言设计意图、详细设计、优缺点的完整技术方案)
- 编译器 (rustc)
  - 将源码编译为 .a, .so, .lib, .dll
  - rustc是跨平台的
  - 支持交叉编译（在当前平台编译出的应用可直接在其他平台运行）
  - 使用LLVM作为编译器后端， 较好的代码生成和优化
  - rustc是自举的
  - rustc错误信息友好
- 核心库
  - 核心库是标准库的基础， 定义的是Rust语言的核心
  - 不依赖于操作系统、网络相关的库
  - 不知道堆分配
  - 不提供并发和I/O
  - `#![no_std]`
  - 应用场景: 嵌入式开发
- 标准库
- 包管理器
  - 按一定规则组织的多个rs文件编译后得到一个包(crate)
  - 社区第三方包发布地址: crates.io, 相关文档发布在: docs.rs
  - 包管理器 cargo 管理工作流程
    - 创建项目 cargo new
    - 运行单元测试、基准测试 cargo test
    - 构建发布链接库 cargo build
    - 运行可执行文件 cargo run






## 语句与表达式

- 语句 (产生副作用的表达式)
  - 声明语句 declaration statement
    - 声明各种语言项 (Item)
      - 变量、常量 (let ...)
      - 结构体 struct
      - 函数 fn
      - extern, use
  - 表达式语句 express statement
    - 分号结尾的表达式
    - 总是返回单元类型 () 
      - 单元类型拥有唯一值， 就是它本身
      - 单元类型的概念来自OCmal, 表示 “没有什么特殊的价值”
      - 函数签名不需要指定返回类型
- 表达式 (用于计算求值)



## 变量与绑定

通过`let` 关键字创建变量, `let` 创建的变量一般称为绑定 - `binding`



### 位置表达式和值表达式

- 位置表达式
  - 表示内存位置的表达式
  - 其他语言中一般叫 左值 LValue
  - 可以被赋值 （写操作）
  - 一般有持久的状态
  - 分类
    - 本地变量
    - 静态变量
    - 解应用 (*expr)
    - 数组索引 (expr[expr])
    - 字段引用 (expr.field)
- 值表达式
  - 右值 Rvalue
  - 读操作
  - 代表了临时数据
  - 要么是字面量，要么是创建的临时值



### 所有权与引用

所有权的转移： 当位置表达式出现在值上下文中。 

```rust
let place1 = "hello";
let place2 = place1; 
```



引用：

```rust
let a = [1,2,3];
let b = &a;
```



- 如果不是转移(Move) , 那就是复制 Copy

- 既不Move也不Copy的是引用 (&) 【不够准确， &其实也是Copy， 只不过Copy 的是内存地址】 

- & 引用 可以直接获取表达式的存储单元地址， 即内存位置

- `let b = &a` 不会引起所有权转移， 因为 **使用引用操作符已经将赋值表达式右侧变成了位置上下文** , 只是共享内存地址 【因此， 本质上这里还是Copy， 只不过Copy的是内存地址】 

- 要获取可变引用， 必须先声明可变绑定

  ```rust
      let mut c = vec![1,2,3];
      let d = &mut c;
      println!("{:?}", d);
  ```

- 值表达式在位置上下文求值时会被创建临时值

  ```rust
  let e = &42;
  assert_eq!(42, *e);
  ```

  字面量42属于值表达式， 通过引用操作符， 相当于值表达式在位置上下文求值. 

  编译器为 `let e = &42; ` 生成的临时值的示意代码：

  ```rust
  let mut _0: &i32;
  let mut _1: i32;
  _1 = const 42i32;
  _0 = &_1;
  ```






## 函数与闭包

### 函数指针 **fn**

函数作为参数、作为返回值

```rust
pub fn math(op: fn(i32, i32) -> i32, a: i32, b: i32) -> i32 {
  op(a, b)
}

fn sum(a: i32, b: i32) -> i32 {
  a + b
}

fn product(a: i32, b: i32) -> i32 {
  a * b
}

fn main(){
  let a = 2; 
  let b = 3;
  assert_eq!(math(sum, a, b), 5);
  assert_eq!(math(product, a, b), 6);
}
```





### 函数与闭包的区别

闭包可以捕获外部变量， 函数不可以。 



###  闭包作为函数参数和返回值 Fn

