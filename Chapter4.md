## trait

### 成员方法
- self，变量名，当前这个，实现了该trait,的具体类型
- `instance.fn()`
- Self，类型名
- self指定类型 
- self : Self类型的各种包装
- `impl trait for type`（为具体类型&type增加成员方法）
- `impl type` 匿名impl，实现类型的内在方法
- `impl trait for dyn trait`（self是一个trait object胖指针，为该指针类型增加成员方法）

> *Note：*
> - 第一个参数名不是self，不能采用小数点语法

### 静态方法
- 第一个参数不是self
- 函数名重名导致自动deref出错的问题
- 不需要static关键字

### 扩展方法
- 利用trait给其他类型扩展方法

> *Note：*
> - `trait | type` 至少一者与trait处于同一crate
> - 匿名impl必须与type在同一crate
> - trait不能直接作为实例变量、参数、返回值（编译期大小不确定）

### trait约束和继承
- 泛型约束：`<T : TraitName>`，要求泛型参数实现该trait
- where子句
- `trait Derived : othTrait` <==> `trait Derived where Self : othTrait`

### Derive
- 自动`impl trait`
- `#[derive(traits...)]`

### trait别名

### 常见trait
#### Display和Debug
- `{}: Display`
- `{:?}, {:#?}: Debug`  

> *Note：*     
> - Display不提供自动derive
> - Display如何格式化取决于自己
> - 默认实现`std::string::ToString`，包含to_String()方法
> - Debug方便调试

#### PartialOrd/Ord
**日后再说**

#### Sized
- 约束
- 编译器推导

#### Default
- 相当于提供一个默认值