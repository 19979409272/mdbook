## 函数

### 简介
- fn
- ->返回类型
- return可选
- 参数列表模式解构（Chapter6）
- 头等公民
> *Note：*
> - 不写返回类型默认返回unit
> - 可嵌套item => 避免污染命名空间
> - 每个函数具有自己独特的类型（即使特征相同）
```rust
// add1 add2 fn((i32, i32)) -> i32
let mut func = add1;
func = add2;  //error

// func转为通用fn类型
// as 
let mut func = add1 as fn((i32, i32))->i32;
// 显示类型标记
let mut func : fn((i32, i32))->i32 = add1;
```

### 发散函数
- panic!()及基于它的函数
- 发散类型（可以被转为任意类型）

### main函数
- 单独API处理传递参数及返回状态码

### const fn
- 编译期执行
- 编译期常量

### 函数递归调用
