<!-- TOC -->

- [let、const](#letconst)
- [Proxy](#proxy)

<!-- /TOC -->

# let、const

1. `var` 和函数存在变量提升，会将声明提升到作用域顶部，函数作用域提升优先于 `var` 
2. `var` 声明的变量可以在声明之前使用，`let`和`const`因为暂时性死区，在声明之前不可以使用
3. `var` 在全局作用域下声明变量会挂载在`window`，`let`和`const`不会
4. `const`声明的变量在声明之后不可以再进行赋值


# Proxy