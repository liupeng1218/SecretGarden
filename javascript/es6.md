<!-- TOC -->

- [let、const](#letconst)
- [解构赋值](#解构赋值)
  - [数组解构赋值](#数组解构赋值)
    - [默认值](#默认值)
  - [对象解构赋值](#对象解构赋值)
    - [默认值](#默认值-1)
  - [参数解构赋值](#参数解构赋值)
- [函数扩展](#函数扩展)
  - [默认值](#默认值-2)
    - [与解构赋值配合使用](#与解构赋值配合使用)
  - [rest 参数](#rest-参数)
  - [箭头函数](#箭头函数)
- [扩展运算符](#扩展运算符)
  - [应用](#应用)
- [Set](#set)
  - [属性和方法](#属性和方法)
  - [遍历操作](#遍历操作)
- [Map](#map)
  - [属性和方法](#属性和方法-1)
  - [遍历方法](#遍历方法)
  - [转换](#转换)
- [Proxy](#proxy)
  - [拦截方法](#拦截方法)
  - [this 问题](#this-问题)
- [Reflect](#reflect)
  - [拦截方法](#拦截方法-1)
- [Class](#class)
  - [constructor](#constructor-function Object() { [native code] }1)
  - [实例](#实例)
  - [取值函数（getter）和存值函数（setter）](#取值函数getter和存值函数setter)
  - [静态方法](#静态方法)
  - [静态属性](#静态属性)
  - [继承](#继承)
    - [super](#super)
      - [函数调用](#函数调用)
      - [super 作为对象](#super-作为对象)
    - [类的 prototype 属性和__proto__属性](#类的-prototype-属性和__proto__属性)
    - [实例的 __proto__ 属性](#实例的-__proto__-属性)

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

# Set

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

Set 本身是一个构造函数，用来生成 Set 数据结构。

```JS
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

Set 函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

```JS
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
const set = new Set(document.querySelectorAll('div'));
set.size // 56

// 类似于
const set = new Set();
document
 .querySelectorAll('div')
 .forEach(div => set.add(div));
set.size // 56
```

**可用于数组去重**

```JS
// 去除数组的重复成员
[...new Set(array)]

```

## 属性和方法

Set 结构的实例有以下属性。

- Set.prototype.constructor：构造函数，默认就是 Set 函数。
- Set.prototype.size：返回 Set 实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

- Set.prototype.add(value)：添加某个值，返回 Set 结构本身。
- Set.prototype.delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
- Set.prototype.has(value)：返回一个布尔值，表示该值是否为 Set 的成员。
- Set.prototype.clear()：清除所有成员，没有返回值。

## 遍历操作

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- Set.prototype.keys()：返回键名的遍历器
- Set.prototype.values()：返回键值的遍历器
- Set.prototype.entries()：返回键值对的遍历器
- Set.prototype.forEach()：使用回调函数遍历每个成员

# Map

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

```JS
const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```

作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

```JS
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```

任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构都可以当作 Map 构造函数的参数。这就是说，Set 和 Map 都可以用来生成新的 Map。

如果对同一个键多次赋值，后面的值将覆盖前面的值。
如果读取一个未知的键，则返回 undefined。

## 属性和方法

Map 结构的实例有以下属性。

- size 属性返回 Map 结构的成员总数。

Map 结构的实例有以下方法。

- set 方法设置键名 key 对应的键值为 value，然后返回整个 Map 结构。如果 key 已经有值，则键值会被更新，否则就新生成该键。
- get 方法读取 key 对应的键值，如果找不到 key，返回 undefined。
- has 方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
- delete 方法删除某个键，返回 true。如果删除失败，返回 false。
- clear 方法清除所有成员，没有返回值。

## 遍历方法

Map 结构原生提供三个遍历器生成函数和一个遍历方法。Map 的遍历顺序就是插入顺序。

- Map.prototype.keys()：返回键名的遍历器。
- Map.prototype.values()：返回键值的遍历器。
- Map.prototype.entries()：返回所有成员的遍历器。
- Map.prototype.forEach()：遍历 Map 的所有成员。

## 转换

1. Map 转为数组，使用扩展运算符（...）。

```JS
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

2. 数组 转为 Map，将数组传入 Map 构造函数，就可以转为 Map

```JS
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
```

3. Map 转为对象,如果所有 Map 的键都是字符串，它可以无损地转为对象。如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

```JS
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

4. 对象转为 Map

```JS
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```

# Proxy

Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例。

```JS
var proxy = new Proxy(target, handler);
```

new Proxy()表示生成一个 Proxy 实例，target 参数表示所要拦截的目标对象，handler 参数也是一个对象，用来定制拦截行为。

```JS
var proxy = new Proxy({}, {
  get: function(target, propKey) {
    return 35;
  }
});

proxy.time // 35
proxy.name // 35
proxy.title // 35
```

Proxy 接受两个参数。第一个参数是所要代理的目标对象（上例是一个空对象），即如果没有 Proxy 的介入，操作原来要访问的就是这个对象；第二个参数是一个配置对象，对于每一个被代理的操作，需要提供一个对应的处理函数，该函数将拦截对应的操作。比如，上面代码中，配置对象有一个 get 方法，用来拦截对目标对象属性的访问请求。get 方法的两个参数分别是目标对象和所要访问的属性。可以看到，由于拦截函数总是返回 35，所以访问任何属性都得到 35。

要使得 Proxy 起作用，**必须针对 Proxy 实例**（上例是 proxy 对象）进行操作，而不是针对目标对象（上例是空对象）进行操作。

**如果 handler 没有设置任何拦截，那就等同于直接通向原对象。**

## 拦截方法

1. get 方法用于拦截某个属性的读取操作，可以接受三个参数，依次为目标对象、属性名和 proxy 实例本身（严格地说，是操作行为所针对的对象），其中最后一个参数可选。

```JS
var person = {
  name: "张三"
};

var proxy = new Proxy(person, {
  get: function(target, propKey) {
    if (propKey in target) {
      return target[propKey];
    } else {
      throw new ReferenceError("Prop name \"" + propKey + "\" does not exist.");
    }
  }
});

proxy.name // "张三"
proxy.age // 抛出一个错误
```

**如果一个属性不可配置（configurable）且不可写（writable），则 Proxy 不能修改该属性，否则通过 Proxy 对象访问该属性会报错。**

2. set 方法用来拦截某个属性的赋值操作，可以接受四个参数，依次为目标对象、属性名、属性值和 Proxy 实例本身，其中最后一个参数可选。

```JS
let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }

    // 对于满足条件的 age 属性以及其他属性，直接保存
    obj[prop] = value;
  }
};

let person = new Proxy({}, validator);

person.age = 100;

person.age // 100
person.age = 'young' // 报错
person.age = 300 // 报错
```

**注意，如果目标对象自身的某个属性，不可写且不可配置，那么 set 方法将不起作用。**

3. apply 方法拦截函数的调用、call 和 apply 操作。apply 方法可以接受三个参数，分别是目标对象、目标对象的上下文对象（this）和目标对象的参数数组。

```JS
var target = function () { return 'I am the target'; };
var handler = {
  apply: function () {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);

p()
// "I am the proxy"
```

4. has 方法用来拦截 HasProperty 操作，即判断对象是否具有某个属性时，这个方法会生效。典型的操作就是 in 运算符。has 方法可以接受两个参数，分别是目标对象、需查询的属性名。
5. construct 方法用于拦截 new 命令。construct 方法可以接受两个参数，target：目标对象，args：构造函数的参数对象
6. deleteProperty 方法用于拦截 delete 操作，如果这个方法抛出错误或者返回 false，当前属性就无法被 delete 命令删除。
7. defineProperty 方法拦截了 Object.defineProperty 操作。
8. getOwnPropertyDescriptor 方法拦截 Object.getOwnPropertyDescriptor()，返回一个属性描述对象或者 undefined。
9. getPrototypeOf 方法主要用来拦截获取对象原型
10. isExtensible 方法拦截 Object.isExtensible 操作。
11. ownKeys 方法用来拦截对象自身属性的读取操作。
12. preventExtensions 方法拦截 Object.preventExtensions()。该方法必须返回一个布尔值，否则会被自动转为布尔值。
13. setPrototypeOf 方法主要用来拦截 Object.setPrototypeOf 方法。
14. Proxy.revocable 方法返回一个可取消的 Proxy 实例。

## this 问题

虽然 Proxy 可以代理针对目标对象的访问，但它不是目标对象的透明代理，即不做任何拦截的情况下，也无法保证与目标对象的行为一致。
主要原因就是在 Proxy 代理的情况下，目标对象内部的 this 关键字会指向 Proxy 代理。有些原生对象的内部属性，只有通过正确的 this 才能拿到，所以 Proxy 也无法代理这些原生对象的属性。

在处理该方面属性时，this 绑定原始对象，就可以解决这个问题。

```JS
const target = new Date('2015-01-01');
const handler = {
  get(target, prop) {
    if (prop === 'getDate') {
      return target.getDate.bind(target);
    }
    return Reflect.get(target, prop);
  }
};
const proxy = new Proxy(target, handler);

proxy.getDate() // 1
```

# Reflect

Reflect 对象与 Proxy 对象一样，也是 ES6 为了操作对象而提供的新 API。Reflect 对象的设计目的有这样几个。

1. 将 Object 对象的一些明显属于**语言内部的方法**（比如 Object.defineProperty），放到 Reflect 对象上。现阶段，某些方法同时在 Object 和 Reflect 对象上部署，未来的新方法将只部署在 Reflect 对象上。也就是说，从 Reflect 对象上可以拿到语言内部的方法。
2. **修改某些 Object 方法的返回结果，让其变得更合理**。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而 Reflect.defineProperty(obj, name, desc)则会返回 false。
3. **让 Object 操作都变成函数行为**。某些 Object 操作是命令式，比如 name in obj 和 delete obj[name]，而 Reflect.has(obj, name)和 Reflect.deleteProperty(obj, name)让它们变成了函数行为。
4. Reflect 对象的方法与 Proxy 对象的方法一一对应，只要是 Proxy 对象的方法，就能在 Reflect 对象上找到对应的方法。这就让 Proxy 对象可以方便地调用对应的 Reflect 方法，完成默认行为，作为修改行为的基础。也就是说，**不管 Proxy 怎么修改默认行为，你总可以在 Reflect 上获取默认行为**。

Reflect 的静态方法与 Proxy 对象一一对应，其对应操作的是`target`对象

## 拦截方法

**如果第一个参数不是对象，Reflect 拦截会报错。**

1. Reflect.get 方法查找并返回 target 对象的 name 属性，如果没有该属性，则返回 undefined。

```JS
var myObject = {
  foo: 1,
  bar: 2,
  get baz() {
    return this.foo + this.bar;
  },
}
var myReceiverObject = {
  foo: 4,
  bar: 4,
};
Reflect.get(myObject, 'foo') // 1
Reflect.get(myObject, 'bar') // 2
Reflect.get(myObject, 'baz') // 3
Reflect.get(myObject, 'baz', myReceiverObject) // 8
```

**如果 name 属性部署了读取函数（getter），则读取函数的 this 绑定 receiver。**

2. Reflect.set 方法设置 target 对象的 name 属性等于 value。

```JS
var myObject = {
  foo: 1,
  set bar(value) {
    return this.foo = value;
  },
}

myObject.foo // 1

Reflect.set(myObject, 'foo', 2);
myObject.foo // 2
Reflect.set(myObject, 'bar', 3)
myObject.foo // 3

var myReceiverObject = {
  foo: 0,
};
Reflect.set(myObject, 'bar', 1, myReceiverObject);
myObject.foo // 3
myReceiverObject.foo // 1
```

**如果 name 属性设置了赋值函数，则赋值函数的 this 绑定 receiver。**

3. Reflect.has 方法对应 name in obj 里面的 in 运算符。
4. Reflect.deleteProperty 方法等同于 delete obj[name]，用于删除对象的属性。
5. Reflect.construct 方法等同于 new target(...args)，这提供了一种不使用 new，来调用构造函数的方法。
6. Reflect.getPrototypeOf 方法用于读取对象的**proto**属性，对应 Object.getPrototypeOf(obj)。
7. Reflect.setPrototypeOf 方法用于设置目标对象的原型（prototype），对应 Object.setPrototypeOf(obj, newProto)方法。它返回一个布尔值，表示是否设置成功。
8. Reflect.apply 方法等同于 Function.prototype.apply.call(func, thisArg, args)，用于绑定 this 对象后执行给定函数。
9. Reflect.defineProperty 方法基本等同于 Object.defineProperty，用来为对象定义属性。未来，后者会被逐渐废除，请从现在开始就使用 Reflect.defineProperty 代替它。
10. Reflect.getOwnPropertyDescriptor 基本等同于 Object.getOwnPropertyDescriptor，用于得到指定属性的描述对象，将来会替代掉后者。
11. Reflect.isExtensible 方法对应 Object.isExtensible，返回一个布尔值，表示当前对象是否可扩展。
12. Reflect.preventExtensions 对应 Object.preventExtensions 方法，用于让一个对象变为不可扩展。它返回一个布尔值，表示是否操作成功。
13. Reflect.ownKeys 方法用于返回对象的所有属性，基本等同于 Object.getOwnPropertyNames 与 Object.getOwnPropertySymbols 之和。

# Class

ES6 引入了 Class（类）这个概念，作为对象的模板。通过 class 关键字，可以定义类。

ES6 的 class 可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

```JS
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

`constructor` 方法，这就是构造方法，而 this 关键字则代表实例对象
定义 `class` 的方法的时候，前面不需要加上 `function` 这个关键字，直接把函数定义放进去了就可以了

使用的时候，也是直接对类使用 new 命令，跟构造函数的用法完全一致。
构造函数的 `prototype` 属性，在 ES6 的类上面继续存在。事实上，类的所有方法都定义在类的 prototype 属性上面。

```JS
class Point {
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

// 等同于

Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};
```

**类的内部所有定义的方法，都是不可枚举的（non-enumerable）**

## constructor

constructor 方法是类的默认方法，通过 new 命令生成对象实例时，自动调用该方法。**一个类必须有 constructor 方法**，如果没有显式定义，一个空的 constructor 方法会被默认添加。
constructor 方法默认返回实例对象（即 this）

```JS
class Point {
}

// 等同于
class Point {
  constructor() {}
}
```

## 实例

生成类的实例的写法，与 ES5 完全一样，也是使用 new 命令
**类必须使用 new 调用，否则会报错**

实例的属性除非显式定义在其本身（即定义在 this 对象上），否则都是定义在原型上（即定义在 class 上）。

```JS
//定义类
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```

## 取值函数（getter）和存值函数（setter）

与 ES5 一样，在“类”的内部可以使用 get 和 set 关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```JS
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

## 静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上 static 关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```JS
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

**如果静态方法包含 this 关键字，这个 this 指的是类，而不是实例。**

## 静态属性

静态属性指的是 Class 本身的属性，即Class.propName，而不是定义在实例对象（this）上的属性。

```JS
class Foo {
}

Foo.prop = 1;
Foo.prop // 1
```

## 继承

Class 可以通过 extends 关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

```JS
class Point {
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

子类必须在 constructor 方法中调用 super 方法，否则新建实例时会报错。这是因为子类自己的 this 对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用 super 方法，子类就得不到 this 对象。
**子类的构造函数中，只有调用 super 之后，才可以使用 this 关键字**

### super

super 这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

#### 函数调用

super 作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次 super 函数。

```JS
class A {}

class B extends A {
  constructor() {
    super();
  }
}
```

**作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错。**

#### super 作为对象

1. super 作为对象时，在普通方法中，指向父类的原型对象

```JS
class A {
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2
  }
}

let b = new B();
```

在子类普通方法中通过 super 调用父类的方法时，方法内部的 this 指向当前的子类实例。

```JS
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```

**由于 super 指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过 super 调用的。**

2. super 作为对象时，在静态方法中，指向父类。

```JS
class Parent {
  static myMethod(msg) {
    console.log('static', msg);
  }

  myMethod(msg) {
    console.log('instance', msg);
  }
}

class Child extends Parent {
  static myMethod(msg) {
    super.myMethod(msg);
  }

  myMethod(msg) {
    super.myMethod(msg);
  }
}

Child.myMethod(1); // static 1

var child = new Child();
child.myMethod(2); // instance 2
```

在子类的静态方法中通过super调用父类的方法时，方法内部的this指向当前的子类，而不是子类的实例。

### 类的 prototype 属性和__proto__属性

大多数浏览器的 ES5 实现之中，每一个对象都有__proto__属性，指向对应的构造函数的prototype属性。Class 作为构造函数的语法糖，同时有prototype属性和__proto__属性，因此同时存在两条继承链。

1. 子类的__proto__属性，表示构造函数的继承，总是指向父类。
2. 子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。


### 实例的 __proto__ 属性

子类实例的__proto__属性的__proto__属性，指向父类实例的__proto__属性。也就是说，子类的原型的原型，是父类的原型。