# 模式解构

## 简介
- let语句中，`=`左边的就是模式，右边是需要被解构的内容
```rust
// 构造，三个元素组合到一起
let tuple = (1_i32, false, 3f32);
// 相反，一个组合的数据结构，拆解，分成三个不同变量
// 模式整体是一个变量名，其中引入了三个变量，分别绑定tuple的三个成员，模式整体与被解构内容类型一致
let (head, center, tail) = tuple;
```
- 构造和解构遵循类似的语法

## match`
- exhaustive-完整的匹配
- `_`
- enum上游依赖问题
- `[non_exhaustive]`

### `_`
- 作用：
    - 占位符，匹配到，但忽略
    - `_`：忽略绑定，直接调用析构函数
    - `let _y = x`：x的所有权转移至_y
    > *Note：*
    >   - 不能作为变量名使用
    >   - 匹配一个元素
    >   - `..`匹配单个|多个元素
    

### match也是表达式
- match表达式（非`;`）
- match每个分支是表达式
- 每个分支需要具备同样的类型（要么`{}`包括，要么逗号分隔
- 使用 `|` 匹配
- 使用范围作匹配条件

### Guards
- 匹配看守（match guards)
- `var: if condition => {code block} | expression,`
- 不保证不同分支重叠问题

### 变量绑定
- `new_var: @ pattern => {code block} | expression,`
- 使用`|`需要保证每个`condition`绑定这个`new_var`

### ref和mut
- ref: 绑定被匹配对象的引用，避免所有权转移
    > *Note：*
    > - `ref`是模式的一部分，只能出现在 `=` 左边
    > - `&` 是借用运算符，是表达式的一部分，只能出现在 `=` 右边
    > - 检测类型
    >   - `fn type_id(_: ()) {}`检测类型
    >   - `std::intrinsics::type_name::<T>()`检测类型
- mut：修饰的变量绑定有修改数据的能力
- 重新绑定：mut修饰的变量绑定可重新绑定到其他同类型的变量
- "重新绑定"与"变量遮蔽"
- &mut型引用：
```rust
// mut x 的mut：代表变量x本身可变，因此能重新绑定（指针的指向能变化）
// &mut i32：修饰的指针，代表这个指针对于指向的内存具有修改能力
let mut x: &mut i32;
```

## if-let和while-let
- if - let ：`if let PATTERN = EXPRESSION { BODY } [else]`
```rust
// Some(value) 与 x: Option<i32>的成员Some<i32>进行模式匹配
// 如果Option状态为None，则不执行if代码块[如果有else分支，则执行else分支]
// 避免了对None值进行操作
// 对于类型参数是指针类型非常有用
if let Some(value) = x {
    //对x内的值操作(value)
} else {
    println!("if分支不匹配");
}
```
- while - let
- `|`
- 变量绑定

    > *Note：*
    > - match一定要完整匹配
    > - if-let只匹配指定分支

## 函数和闭包参数做模式解构
- 函数接受一个结构体参数，可以直接对参数做模式解构