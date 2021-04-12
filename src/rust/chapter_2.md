# 表达式与语句

## 语句
- ;
- 不产生值（ :() ）
- {语句} => 表达式

## 表达式
- 不带;
- 左值，右值

### 运算表达式
-   ‘+ - * / %’
- 连续比较问题
- 位运算
- 逻辑运算
    - 短路现象 （ 逻辑与 | 逻辑或 ）

### 赋值表达式
- 复制或移动问题（Chapter11）
- 类型：unit（空的tuple（））
    - => 不可连续赋值 
    - => 避免条件判断 == 写成 = ，条件表达式类型必须bool类型
- 组合赋值表达式（+=等）
    > *Note：*
    > - 不支持 `++`及`--` 同 python

### 语句块表达式
- 按照顺序执行语句块内语句，并返回最后一个表达式类型

### if-else
- 语句块必须有大括号
- if condition不需要括号
- if,else类型必须一致，else分支省略，默认其为()

### loop
- 无限死循环
- 生命周期标识符， 'a:loop { break 'a} 控制跳出层数
- 作为表达式，`break value; `
- 发散类型

### while
- loop 与 while的区别
```rust
let x;
loop {x = 1; break;} //静态分析得出x=1必然在println！之前执行过
println!("{}", x); //可以通过编译

let x;
while true {x=1; break;} //while语句的执行条件跟表达式在运行阶段的值有关，因此不确定x是否初始化
println!("{}", x); //不能通过编译（use of possibly uninitialized variable）
```

### for
- 其他语言的for-each
- `for i in container {}`
- 利用迭代器遍历
- 支持自定义类型（迭代器，容器）