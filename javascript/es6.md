<!-- TOC -->

- [let、const](#letconst)
- [解构赋值](#%e8%a7%a3%e6%9e%84%e8%b5%8b%e5%80%bc)
  - [数组解构赋值](#%e6%95%b0%e7%bb%84%e8%a7%a3%e6%9e%84%e8%b5%8b%e5%80%bc)
    - [默认值](#%e9%bb%98%e8%ae%a4%e5%80%bc)
  - [对象解构赋值](#%e5%af%b9%e8%b1%a1%e8%a7%a3%e6%9e%84%e8%b5%8b%e5%80%bc)
    - [默认值](#%e9%bb%98%e8%ae%a4%e5%80%bc-1)
  - [参数解构赋值](#%e5%8f%82%e6%95%b0%e8%a7%a3%e6%9e%84%e8%b5%8b%e5%80%bc)
- [函数扩展](#%e5%87%bd%e6%95%b0%e6%89%a9%e5%b1%95)
  - [默认值](#%e9%bb%98%e8%ae%a4%e5%80%bc-2)
    - [与解构赋值配合使用](#%e4%b8%8e%e8%a7%a3%e6%9e%84%e8%b5%8b%e5%80%bc%e9%85%8d%e5%90%88%e4%bd%bf%e7%94%a8)
  - [rest 参数](#rest-%e5%8f%82%e6%95%b0)
  - [箭头函数](#%e7%ae%ad%e5%a4%b4%e5%87%bd%e6%95%b0)
- [扩展运算符](#%e6%89%a9%e5%b1%95%e8%bf%90%e7%ae%97%e7%ac%a6)
  - [应用](#%e5%ba%94%e7%94%a8)
- [Set、Map](#setmap)
- [Proxy、Reflect](#proxyreflect)
- [Class](#class)

<!-- /TOC -->

# let、const

1. `var` 和函数存在变量提升，会将声明提升到作用域顶部，函数作用域提升优先于 `var`
2. `let`和`const`只在所在代码块中有效
3. `let`不允许在相同作用域内，重复声明同一变量
4. `var` 声明的变量可以在声明之前使用，`let`和`const`因为暂时性死区，在声明之前不可以使用
5. `var` 在全局作用域下声明变量会挂载在`window`，`let`和`const`不会
6. `const`声明的变量在声明之后不可以再进行赋值

# 解构赋值

## 数组解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值

```JS
let [a, b, c] = [1, 2, 3];
```

- 如果左侧模式变量大于右侧数组，则解构不成功，未匹配到的变量等于`undefined`
- 如果左侧模式小于大于右侧数组，则为不完全结构
- 如果右侧不是可遍历结构，则会报错

### 默认值

解构允许指定默认值

```JS
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于 undefined，默认值才会生效。

```JS
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

## 对象解构赋值

对象解构赋值从右侧对象中取与模式相对应的属性，如果没有去到则为`undefined`

```JS
let { bar, foo } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: 'aaa', bar: 'bbb' };
baz // undefined
```

对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

```JS
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"
foo // error: foo is not defined
```

### 默认值

```JS
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x: y = 3} = {};
y // 3

var {x: y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```

默认值生效的条件是，对象的属性值严格等于 undefined。

## 参数解构赋值

函数参数也可以进行结构赋值

```JS
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```

函数 add 的参数表面上是一个数组，但在传入参数的那一刻，数组参数就被解构成变量 x 和 y。对于函数内部的代码来说，它们能感受到的参数就是 x 和 y。

函数参数的解构也可以使用默认值

```JS
function move({x = 0, y = 0} = {}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```

函数 move 的参数是一个对象，通过对这个对象进行解构，得到变量 x 和 y 的值。如果解构失败，x 和 y 等于默认值。

# 函数扩展

## 默认值

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

```JS
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```

默认声明的参数，在函数内不能用`let`和`const`再次声明

### 与解构赋值配合使用

参数默认值可以与解构赋值的默认值，结合起来使用。

```JS
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined 5
foo({x: 1}) // 1 5
foo({x: 1, y: 2}) // 1 2
foo() // TypeError: Cannot read property 'x' of undefined
```

上面代码只使用了对象的解构赋值默认值，没有使用函数参数的默认值。只有当函数 foo 的参数是一个对象时，变量 x 和 y 才会通过解构赋值生成。如果函数 foo 调用时没提供参数，变量 x 和 y 就不会生成，从而报错。通过提供函数参数的默认值，就可以避免这种情况。

```JS
function foo({x, y = 5} = {}) {
  console.log(x, y);
}

foo() // undefined 5
```

上面代码指定，如果没有提供参数，函数 foo 的参数默认为一个空对象。

## rest 参数

ES6 引入 rest 参数（形式为...变量名），用于获取函数的**多余参数**，这样就不需要使用 arguments 对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

```JS
function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

add(2, 5, 3) // 10
```

arguments 对象不是数组，而是一个类似数组的对象。所以为了使用数组的方法，必须使用 Array.prototype.slice.call 先将其转为数组。rest 参数就不存在这个问题，它就是一个真正的数组，数组特有的方法都可以使用。

**rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。**

## 箭头函数

ES6 允许使用“箭头”（=>）定义函数。

```JS
var f = v => v;

// 等同于
var f = function (v) {
  return v;
};

```

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。

```JS
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};

```

1. 函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。
1. 不可以当作构造函数，也就是说，不可以使用 new 命令，否则会抛出一个错误。
1. 不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

# 扩展运算符

扩展运算符（spread）是三个点 `...` 。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。
```JS
console.log(...[1, 2, 3])
// 1 2 3

console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5

[...document.querySelectorAll('div')]
// [<div>, <div>, <div>]
```

主要用于函数调用
```JS
function push(array, ...items) {
  array.push(...items);
}

function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 42
```
扩展运算符与正常的函数参数可以结合使用，非常灵活。

```JS
function f(v, w, x, y, z) { }
const args = [0, 1];
f(-1, ...args, 2, ...[3]);

```

## 应用

扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符

```JS
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]
```

1. 复制数组

数组是对象数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。

```JS
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```
2. 数组合并

```JS
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```

3. 扩展运算符可以与解构赋值结合起来，用于生成数组。

```JS
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```

# Set、Map

# Proxy、Reflect

# Class
