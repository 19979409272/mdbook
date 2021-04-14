# 宏

## 简介 macro
- “卫生宏（hygiene）”
- 调用语法与函数有明显区别
- 实现与外部调用处于不同命名空间，访问范围严格受限，通过参数传入，不能随意在宏内访问和改变外部代码

### 实现编译阶段检查
- **如果用普通函数实现这个功能，不可能实现这样的编译阶段错误检查**
- **日后再来**
```rust
fn main() {
    // invalid reference to argument '0' (no arguments given)
    println!("number1 {} number2 {}");
}
```
- 检查正则表达式的正确性

### 实现编译期计算
- 某些场景，利用宏来完成编译期计算也是一种可行的选择
- `file!(), line!()`

### 实现自动代码生成
> Don't Repeat Yourself
- `add_impl! { usize u8 u16 u32 u64 isize i8 i16 i32 i64 f32 f64} //std::ops::Add trait`

### 实现语法扩展
- `let v = vec![1, 2, 3, 4, 5];`
- 增加表达能力
- 自定义DSL

## 示范型宏
- 方式：
    1. macro_rules!宏
    2. 编译器扩展
- `macro_rules!`
```rust
// 要实现的语法
let counts = hashmap! ['A' => 0, 'C' => 0];
...
```
- `rustc -Z unstable-options --pretty=expanded file.rs`

## 宏1.1