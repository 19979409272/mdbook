## 数组和字符串

### 数组
- `[T ; n]`
- 同类型数字互相赋值
- 入参复制mode

#### 内置方法
1. 比较
2. 遍历（数组切片）

#### 多维数组
- `[[T;m];n]`

#### 数组切片
- `（借用）&array->Slice`（数组切片：数组到指针的安全转换）
- `Slice: &[T]`（指针）
- **对数组没有所有权（不懂，搁置）**
- `size_of::<&[T;n]>() = one point`
- `size_of::<&[T]>() = two point`（携带长度信息）

#### DST和胖指针
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

#### Range
- 