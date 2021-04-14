# 所有权和移动语义

## 所有权
- 每个值都由一个变量管理，这个变量就是这个值，这块内存的所有者
- 每个值在一个时间点只有一个管理者
- 变量作用域结束，变量及值将被销毁

## 移动语义
- 一个变量把它拥有的值转移给另一个变量，称“所有权的转移
- 赋值、函数返回、函数调用
- 简单的memcpy
- **所有类型的默认语义**
- 降低了语言复杂度，不可能执行用户自定义的代码，没有任何内存安全风险，满足异常安全
> *Note：*
> - “语义”只是规定了编译器接受什么样的代码，以及执行后的效果用怎样的思维模型去理解
> - 编译器有权在不改变语义的情况下做任何有利于执行效率的优化

## 赋值语义
- 凡事实现`std::marker::Copy trait`的类型，都是copy语义
- 基本类型
- 为自定义类型实现`Copy trait`
- `Copy inherit Clone`
- derive attribute `#[derive(Copy, Clone)]`

## Box类型
- 拥有所有权的指针
- `Box::new(T(value: 1))`：装箱到heap
- Rust要求使用前必须初始化，所以Box<T> / &T / &mut T一定指向某个对象
- `box`语法：`box T{value: 1};` 通用的装箱语法

## Clone Vs. Copy

### Copy的含义
- `std::marker`：特殊的trait（**Copy、Send、Sized、Sync**），
- impl后给类型标记，影响编译器的静态检查以及代码生成

### Copy的实现条件
> POD（Plain Old Data）：普通的与几十年前C兼容的，可以使用memcpy原始函数操作的类型
- 所有成员都实现Copy trait
- POD,且没有自定义析构函数
- `impl Copy for`基本类型，共享借用指针& 
- `not impl Copy for` Box、Vec、可写借用指针、&mut等

> *Note：*
> - Box String Vec等不能实现memcpy,都不属于POD

### Clone的含义
- `std::clone::Clone`
- 无前提条件
```rust
pub trait Clone : Sized { ///////trait 不是实现Sized约束
    fn clone(&self) -> Self; //没有默认实现
   
   //默认实现clone_from取决于clone实现
    fn clone_from(&mut self, source: &Self) {
        *self = source.clone() 
    }
}
```
- 基于语义的复制，具体作用取决于具体类型
- Box类型则是"深复制"，拷贝的不是内存地址，Rc类型引用计数+1
- 如果实现了Copy（浅复制），则clone也应是与copy语义相容，memcpy

### 自动derive
- 对于泛型参数，自动derive会自动添加一个T: Copy约束
- `impl Clone` 调用每个成员的clone()方法

## 析构函数
- 在对象消亡之前由编译器自动调用（编译期间）
- 析构函数不仅可管理内存资源，还可以管理文件（关闭文件）、锁、socket等
- `impl std::ops::Drop`
```rust
trait Drop {
    fn drop(&mut self);
}
```

### 主动析构
- 主动调用析构函数是非法的
- 提前终止生命周期应调用 `std::mem::drop` 函数
```rust
#[inline]
//move语义
pub fn drop<T> (_x : T) { }
```
- 变量遮蔽（遮蔽变量与被遮蔽变量是两个不同的变量）不会导致变量生命周期提前结束
```rust
let x = 1;
println!("{:p}", &x); //两者地址不同

let x = 1;
println!("{:p}", &x)
```
- `_`忽略绑定，直接析构

### Drop VS. Copy
- 如果类型可以memcpy复制，且没有内存安全问题（无自定义析构函数），才允许`impl Copy`
- **带有析构函数的类型不可能是Copy**（Copy实现浅复制，有内存安全问题 | 基本类型创建在栈上）
- 

### 析构标记
- 析构函数的具体调用时机还和运行时的情况相关，编译器会在每个路径上插入标记，来标记该对象的析构函数是否被调用
- 编译阶段确定生命周期，执行阶段根据情况调用