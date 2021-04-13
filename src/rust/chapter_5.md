# 数组和字符串

## 数组
- `[T ; n]`
- 同类型数字互相赋值
- 入参复制mode

### 内置方法
1. 比较
2. 遍历（数组切片）

### 多维数组
- `[[T;m];n]`

### 数组切片
- `（借用）&array->Slice`（数组切片：数组到指针的安全转换）
- `Slice: &[T]`（指针）
- **对数组没有所有权（不懂，搁置）**
- `size_of::<&[T;n]>() = one point`
- `size_of::<&[T]>() = two point`（携带长度信息）

### DST和胖指针
- `fat pointer -> DST`
- `Dynamic Sized Type`：编译期无法确定占用空间
- `[T]:DST；str:DST`
- `&[T]:fat pointer -> [T]；&str: fat pointer -> str`
- `::size_of::<DST>() = undefined`
- `::size_of::<fat pointer>() = defined`

*Note：*    
1. 只能通过指针间接*创建和操作*DST
2. 局部变量和函数参数必须在编译期知道其大小（未实现Sized约束）, 所以不能是DST
3. enum不能包含DST
4. struct只能最后一个元素为DST，此时struct就是一个DST
5. 胖指针的设计，避免参数传递退化为裸指针，丢失长度信息的问题

### Range
- `begin..end` 语法糖，范围：`[)`
- `->std::ops::Range<_>`，没有实现 `Copy trait`
- 实现了`Iterator trait`
- Range其他类型:
    - Rangefrom：`begin..`
    - RangeTo：`..end`
    - full range：`..` 
- `..=` 范围：'[]'
- **用来数组索引（部分切片）：**
- `arr[..] : [i32] //arr : [i32; n] but arr[..] : DST`
```rust
let arr : [i32; 5] = [1, 2, 3, 4, 5];
let fat_arr : &[i32] = &arr[..];  //fat_arr is fat pointer
let oth_fat_arr : &[i32] = &fat_arr[2..]; //Rangefrom &fat_arr[2..] = &&[i32][2..]   
//&(&[i32]) auto deref -> &[i32 ; n] => &[i32 ; n][2..] = &[i32]

```

### 边界检查
- 运行期边界检查
- trait:
    - 索引写：`std::ops::Index trait`
    - 索引读：`std::ops::IndexMut trait`
- usize作索引
- 不推荐大量使用索引（正常的索引op会执行一次边界检查，推荐使用迭代器方法）

## 字符串

### &str
- `str : DST`
- `&str : fat pointer` 字符串切片
- 对于一块字符串区间的借用，对指向的**内存空间**没有所有权，无法扩大引用的范围
- utf-8
- 不能使用O(1)复杂度的索引
- `s.chars().nth(n)`
- `&str =` 指向字符串头部片段的指针+长度

### String
- 权力不同于&str
- 可以扩大引用的范围
- 堆
- 内部实现类似：`std::Vec<u8>`
- `impl Deref<Target=str> for String`

    > *Note：*
    > - `not impl Copy for String`