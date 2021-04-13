# 模式解构

## 简介
- let语句中，`=`左边的就是模式，右边是需要被解构的内容
```rust
// 构造，三个元素组合到一起
let tuple = (1_i32, false, 3f32);
// 相反，一个组合的数据结构，拆解，分成三个不同变量
// 模式引入了三个变量，分别绑定tuple的三个成员
let (head, center, tail) = tuple;
```
- 构造和解构遵循类似的语法

## match`
- exhaustive-完整的匹配
- `_`
- 上游依赖
- `[non_exhaustive]`

## `_`
- 作用：
    - 占位符，匹配到，但忽略
    - 忽略绑定
    - 所有权转移    
    > *Note：*
    >   - 不能作为变量名使用
    >   - 匹配一个元素
    >   - `..`匹配单个|多个元素

## match也是表达式
- match表达式（非`;`）
- match每个分支是表达式
- 每个分支需要具备同样的类型（要么`{}`包括，要么逗号分隔
- 使用`|`匹配
- 使用范围作匹配条件

## Guards
- 匹配看守（match guards)
- `var: if pattern => {code block} | expression,`
- 不保证不同分支重叠问题

## 变量绑定
- `new_var: @ pattern => {code block} | expression,`
- 

## ref和mut
