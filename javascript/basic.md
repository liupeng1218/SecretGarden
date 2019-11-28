<!-- TOC -->

- [数据类型](#%e6%95%b0%e6%8d%ae%e7%b1%bb%e5%9e%8b)
  - [原始类型](#%e5%8e%9f%e5%a7%8b%e7%b1%bb%e5%9e%8b)
  - [对象类型](#%e5%af%b9%e8%b1%a1%e7%b1%bb%e5%9e%8b)
  - [检测](#%e6%a3%80%e6%b5%8b)
    - [typeof](#typeof)
    - [instanceof](#instanceof)
      - [实现 instanceof](#%e5%ae%9e%e7%8e%b0-instanceof)
    - [Object.prototype.toString.call()](#objectprototypetostringcall)
  - [类型转换](#%e7%b1%bb%e5%9e%8b%e8%bd%ac%e6%8d%a2)
    - [强制转换](#%e5%bc%ba%e5%88%b6%e8%bd%ac%e6%8d%a2)
    - [隐式转换](#%e9%9a%90%e5%bc%8f%e8%bd%ac%e6%8d%a2)
      - [四则运算](#%e5%9b%9b%e5%88%99%e8%bf%90%e7%ae%97)
      - [== 和 ===](#%e5%92%8c)
- [闭包](#%e9%97%ad%e5%8c%85)
- [this](#this)
- [new](#new)

<!-- /TOC -->

**本章节记录 JavaScript 中部分常用基础概念，仅用于整理记录**
**基础内容可以参考[MDN](https://developer.mozilla.org)和阮一峰老师的[网道](https://wangdoc.com/)**

# 数据类型

JS 有七种内置类型，六种原始类型和一种对象类型`Object`

## 原始类型

原始类型：`null`，`undefined`，`boolean`，`string`，`number`，`symbol`

1. `null`不是对象，`typeof null`输出`object`是 js 历史版本中造成的错误判断
2. `NaN`也属于`number`，`NaN`和`NaN`不相等
3. 直接对原始类型进行操作，会先被转换为对应的对象类型进行操作，最后销毁生成的实例
4. `number`类型是浮点型，浮点计算会出现不精确的情况，`0.1+0.2!=0.3`

## 对象类型

- 除了原始类型都是对象类型
- 原始类型存储的是值，对象类型存储的是地址
- 原始类型是值传递，直接复制在堆中的值。对象类型是引用传递，复制对象在堆中的内存地址值，指向在栈中的同一内容

## 检测

### typeof

- `typeof` 对于原始类型，除了 `null` 都可以显示正确的类型
- `typeof null`会显示`object`
- `typeof`对于对象，除了函数都会显示`object`
- `typeof`对于函数，显示`function`

### instanceof

instanceof 可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的 prototype，只要处于原型链中，就判断为`true`

```JS
const Person = function() {}
const p1 = new Person()
p1 instanceof Person // true
```

#### 实现 instanceof

```JS
    function myInstanceof(ex, obj) {
      // 基本类型直接返回false
      if (typeof ex === 'null' || typeof ex !== 'object') {
        return false
      }
      let exProto = Object.getPrototypeOf(ex)
      while (true) {
        // 原型链尽头
        if (exProto === null) {
          return false
        }
        // 找到相同原型
        if (exProto === obj.prototype) {
          return true
        }
        // 上溯原型
        exProto = Object.getPrototypeOf(exProto)
      }
    }
```

### Object.prototype.toString.call()

使用`Object.prototype.toString.call()`方法可以返回字符串`[object 对象类型\]`，可以检测所有内置对象的类型。

**无法判断自定义对象的类型。**

## 类型转换

### 强制转换

- 转换为布尔值：`Boolean()`
- 转换为数字：`Number()`、`ParseInt`和`ParseFloat`
- 转换为字符串：调用`String()`时，如果有`toString()`方法则会调用该方法

| 原始值                                 | 转换目标 | 结果                                               |
| -------------------------------------- | -------- | -------------------------------------------------- |
| number                                 | 布尔值   | 除了 0,-0,NaN 都为 true                            |
| string                                 | 布尔值   | 除了空字符串都为 true                              |
| null、undefined                        | 布尔值   | false                                              |
| 引用类型                               | 布尔值   | true                                               |
| number                                 | 字符串   | 5=>"5"                                             |
| boolean、函数、Symbol、null、undefined | 字符串   | true=>"true"                                       |
| 数组                                   | 字符串   | [1,2]=>"1,2"                                       |
| 对象                                   | 字符串   | "[object Object]"                                  |
| string                                 | 数字     | "1"=>1 "a"=>NaN                                    |
| 数组                                   | 数字     | 空数组为 0,只有一个元素且为数值转为数值,其他为 NaN |
| null                                   | 数字     | 0                                                  |
| 除数组的引用类型、undefined            | 数字     | NaN                                                |
| Symbol                                 | 数字     | 抛错                                               |

### 隐式转换

#### 四则运算

加法运算符

- 运算中其中一方为字符串，那么就会把另一方也转换为字符串
- 如果其中一方是字符串或数字，会将另一方转换为字符串或数字（与预期类型相同）
- 如果双方都不是字符串或数字，会默认转为数字

除了加法的运算符，只要其中一方是数字，那么另一方就会被转为数字

#### == 和 ===

`==`对比，如果类型不同，会先进行类型转换，在进行对比

1. 两种类型相同，比值
2. 是否在对比`null`和`undefined`，是则返回`true`
3. `string`和`number`比较，会将`string`转为`number`
4. `boolean`转换，会将`boolean`转为`number`
5. 引用类型和值类型比较，会先将引用类型转换为值类型，与何种值类型比较，引用类型就转为该类型，布尔类型会转为数值类型，都转换为值类型之后，`Number(值类型)==Number(值类型)`进行比较

`===`对比，直接对比类型和值

**对象转值类型流程**
对象转原始类型，会调用内置的`[ToPrimitive]`函数，该函数逻辑为

1. 如果有`Symbol.toPrimitive`方法，优先调用返回
2. 调用`valueOf`，如果转换为原始类型，则返回
3. 调用`toString`，如果转换为原始类型，则返回
4. 都没有返回原始类型，报错

# 闭包

什么是闭包？

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)函数与对其状态即词法环境的引用共同构成闭包。也就是说，闭包可以让你从内部函数访问外部函数作用域。在 JavaScript，函数在每次创建时生成闭包。
JavaScript 高级程序设计：闭包是指有权访问另外一个函数作用域中的变量的函数

```JS
// 函数 A 内部有一个函数 B，函数 B 可以访问到函数 A 中的变量，那么函数 B 就是闭包。
function A() {
  let a = 1
  window.B = function () {
      console.log(a)
  }
}
A()
B() // 1
```

**作用：**
闭包可以暂存函数执行时的数据状态
实现封装和管理私有变量和方法

**缺点：**
闭包会在内存中保留函数的执行环境，过度使用会造成内存泄漏

> 循环中使用闭包解决 `var` 定义函数的问题

```JS
for (var i = 1; i <= 5; i++) {
  ;(function(j) {
    setTimeout(function timer() {
      console.log(j)
    }, j * 1000)
  })(i)
} // 闭包

for (var i = 1; i <= 5; i++) {
  setTimeout(
    function timer(j) {
      console.log(j)
    },
    i * 1000,
    i
  )
}  // setTimeout 第三个参数

for (let i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i)
  }, i * 1000)
}  // let
```

# this

> 如何正确判断 this？箭头函数的 this 是什么？

this 只与调用场景有关

1. 直接调用：this 指向 window
2. 对象调用：this 指向对象
3. new 构造：指向构造出来的对象
4. bind|call|apply：指向方法的第一个参数
5. 箭头函数：没有 this，取决于包裹箭头函数的第一个普通函数的 this

**不管我们给函数 bind 几次，fn 中的 this 永远由第一次 bind 决定**

多种场景混合

1. new 优先级最高
2. bind|call|apply 其次
3. 对象调用
4. 直接调用

**箭头函数 this 一旦被绑定就不会改变**

# new

> new 的原理？和字面量有什么区别？

调用 new

1. 创建一个空对象
2. this 指向对象
3. 对象链接到函数原型
4. 返回对象

使用字面量的方式创建对象（无论性能上还是可读性）。使用 `new Object()` 的方式创建对象需要通过作用域链一层层找到 `Object
