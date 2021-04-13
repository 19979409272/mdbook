# 深入类型系统

## 代数类型系统（ADT）
- ”基数”（cardinality）：一个类型所有取值的可能性   
    - Cardinality(unit) = 1 : ()
    - Cardinality(bool) = 2 : true false
    - Cardinality(i32)  = $2^{32}$
- “同构”：复合类型的内部类型相同，基数一样，它们携带的信息量一样
- ```Cardinality(Option<T>) = Cardinality(T) + 1```
- 类型类比代数的变量，类型间的组合类比代数的运算
- `空enum`: 0，`空struct` | `unit`: 1 
- `enum`：和，`struct`：积，`array`：乘方...

## Never Type
- `Cardinality(T) = 2^bits_of(T)`
- `unit | 空struct` 类比1，所以`bits_of(()) = log(1) = 0`
```rust
//定义HashSet只是将HashMap的“value”指定为unit
pub struct HashSet<T, s = RandomState> {
    map: HashMap<T, (), S>,
}
```
- Never Type的属性：
    - `Cardinality(Never) = 0`
    - `bits_of(Nerver) = log(0) = -∞`
    - 可以被转换成任意类型
    - 根本不可能返回 
    - 运行时不可能存在  
    - 根本不可能执行    
```rust
// continue;语句指定为nerver
// 编译器判断如下：
// 1. else分支类型与if一致
// 2. 执行到else分支，根本不可能对x赋值，会进入下一次循环
// 3. 后面的代码不可能执行
// 一切都在类型系统得到统一
let x: i32 = if cond { 1 } else { continue; }   
```
- !: never 
- `diverging function -> never` 

*Note：*    
- 完整的never type的意义：
    1. **可以使得泛型代码兼容diverging function**
    ```rust
    fn call_fn<T, F: Fn(i32) -> T> (f: F, arg: i32) -> T { f(arg) }
    //不把!当成一个真正意义上的的类型，那么这句话就编译错误，因为只有类型才能替换类型参数
    call_fn(std::process::exit, 0);
    ```
    2. **更好的死代码检查** (**看不懂**)
    ```rust
    let t = std::thread::spawn(|| panic!("nope"));
    t.join().unwrap();
    println!("hello);
    ```
    3. **可以用更好的方式表达“不可能出现的情况”**
        - `type Err = !`

## 再谈Option类型
- **空指针问题**：违背类型系统
> Type is a classification identifying one of various types of data, that determines the possible values for that, the operations that can be done on values of that type, the meaning of the data, and the way values of that type can be stored.

> null实际上是在类型系统上打开了一个缺口，引入了一个必须在运行期特殊处理的特殊“值”。它就像一个全局的、无类型的singleton变量一样，可以无处不在，可以随意与任意指针实现自动类型转换。它让编译器的类型检查在此处失去了意义
- Rust利用ADT将空指针和非空指针区分，分别赋予不同的操作权限，禁止空指针的解引用：编译器和静态检查工具不可能知道一个变量在运行期的“值”，但是，如果一个null从一个“值”上升为一个“类型”，那么静态检查就能发挥作用。
- Option<T>是可空类型`enum Option<T> { None, Some(T) }`
- 无法直接调用T类型的成员函数
    - 拆
    - 成员函数
- **暂时略过**