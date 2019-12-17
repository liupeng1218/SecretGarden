<!-- TOC -->

- [Object](#object)
  - [静态方法](#静态方法)
    - [对象属性模型的相关方法](#对象属性模型的相关方法)
    - [控制对象状态的方法](#控制对象状态的方法)
    - [原型链相关](#原型链相关)
  - [实例方法](#实例方法)
  - [属性描述对象](#属性描述对象)
- [Array](#array)
  - [静态方法](#静态方法-1)
  - [实例方法](#实例方法-1)
- [包装对象](#包装对象)
  - [实例方法](#实例方法-2)
  - [自动转换](#自动转换)
- [Boolean](#boolean)
- [Number](#number)
  - [实例方法](#实例方法-3)
- [String](#string)
  - [实例属性](#实例属性)
  - [实例方法](#实例方法-4)
- [Math](#math)
  - [静态方法](#静态方法-2)
- [Date](#date)
  - [构造函数](#构造函数)
  - [静态方法](#静态方法-3)
  - [实例方法](#实例方法-5)
- [JSON](#json)
  - [静态方法](#静态方法-4)

<!-- /TOC -->

**JavaScript 标准库在使用中已经形成长期记忆，本章内容将常用 API 简要罗列，主要是用于回顾和温习**
**更多内容可以参考[MDN](https://developer.mozilla.org)和阮一峰老师的[网道](https://wangdoc.com/)**

# Object

## 静态方法

- `Object.is`：比较两个值是否相等
- `Object.assign`：将源对象（source）的所有可枚举属性，复制到目标对象（target）。
- `Objcet.keys` 方法的参数是一个对象，返回一个包含对象**自身所有属性名**的数组。
- `Object.getOwnPropertyNames` 方法的参数是一个对象，返回一个包含对象**自身所有属性名**的数组。`Object.getOwnPropertyNames`可以返回**不可枚举的属性名**，`Object.key`只包含可枚举的属性

**Object.getOwnPropertyNames、Objcet.keys 和 for in**

- for...in 循环遍历对象自身的和继承自原型的可枚举属性（不含 Symbol 属性）。
- Object.keys 返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）。
- Object.getOwnPropertyNames 返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）

|                            | 自身可枚举 | 自身不可枚举 | 原型可枚举 | 原型不可枚举 |
| -------------------------- | ---------- | ------------ | ---------- | ------------ |
| for...in                   | 是         | 否           | 是         | 否           |
| Object.keys                | 是         | 否           | 否         | 否           |
| Object.getOwnPropertyNames | 是         | 是           | 否         | 否           |

### 对象属性模型的相关方法

- `Object.getOwnPropertyDescriptor`：获取某个属性的描述对象
- `Object.defineProperty`：通过描述对象，定义某个属性
- `Object.defineProperties`：通过描述对象，定义多个属性

### 控制对象状态的方法

- `Object.preventExtensions`：防止对象扩展
- `Object.isExtensible`：判断对象是否可扩展
- `Object.seal`：禁止对象配置
- `Object.isSealed`：判断对象是否可配置
- `Object.freeze`：冻结一个对象
- `Object.isFrozen`：判断一个对象是否可冻结

### 原型链相关

- `Object.create`：指定原型对象和属性，返回一个新的对象
- `Object.getPrototypeOf`：获取对象的 `Prototype` 对象

## 实例方法

定义在`Object.prototype`对象的方法，成为实例方法，所有`Objcet`实例都基础这些方法

- `Object.prototype.valueOf`：返回对象对应的值
- `Object.prototype.toString`：返回对象对应的字符串形式
- `Object.prototype.hasOwnProperty`：判断某个属性是否为当前对象自身的属性
- `Object.prototype.isPrototypeOf`：判断当前对象是否为另一个对象的原型
- `Object.prototype.propertyIsEnumerable`：判断某个属性是否可枚举

## 属性描述对象

JavaScript 提供一个内部数据结构，用来描述对象的属性，控制他的行为，比如该属性是否可写，可遍历等等。这个内部数据结构称为"属性描述对象"

- value：属性值
- writable：是否可写，默认为 `true`
- enumerable：是否可遍历，默认为 `true`
- configurable：是否可配置，默认为 `true`
- get：该属性的取值函数
- set：该属性的存值函数

# Array

## 静态方法

- `Array.isArray`：返回一个布尔值，表示参数是否为数组
- `Array.from`：将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象，还可以接受第二个参数，作用类似于数组的 map 方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

## 实例方法

- `valueOf`：`valueOf`是所有对象都拥有的方法，数组的`valueOf`返回数组本身
- `toString`：返回数组的字符串形式，使用逗号连接
- `push`/`pop`：在数组末尾添加和删除元素，**改变原数组**
- `shift`/`unshift`：在数组的开头添加和删除元素，**改变原数组**
- `join`：将数组以指定参数为分隔符连接为字符串返回，默认使用逗号分隔
- `concat`：将参数作为数组的新成员添加到原数组尾部
- `reverse`：颠倒数组，**改变原数组**
- `slice`：提取目标数组的一部分
- `split`：删除数组的成员，并在删除位置添加新成员，**改变原数组**
- `sort`：对数组进行排序，**改变原数组**
- `map`：遍历数组的每一个元素，把每一次的执行结果组成一个数组返回
- `forEach`：遍历数组的每一个元素
- `filter`：过滤数组成员，满足条件的元素组成数组返回
- `some/erery`：`some`方法只要数组有一个元素符合条件就返回`true`，`every`方法是所有元素都满足条件则返回`true`
- `reduce`：方法依次处理每一个元素，返回一个累加值
- `indexOf`：返回给定元素在数组中第一次出现的位置，没有改元素返回`-1`

# 包装对象

数值、字符串、布尔值在一定条件下，会自动转为对象，就是原型类型的**包装对象**

```JS
var v1 = new Number(123);
var v2 = new String('abc');
var v3 = new Boolean(true);

typeof v1 // "object"
typeof v2 // "object"
typeof v3 // "object"

v1 === 123 // false
v2 === 'abc' // false
v3 === true // false

```

## 实例方法

- `valueOf`：返回包装对象实例对应的原始类型值
- `toString`：返回对应的字符串形式

## 自动转换

在某些场合，原始类型的值会自动当前包装对象调用，即调用包装对象的属性和方法。这时，引擎会自动将原始类型的值转为包装对象实例，并在使用后立刻销毁实例

```JS
'abc'.length // 3
```

# Boolean

Boolean 对象是 JavaScript 的三个包装对象之一。作为构造函数，它主要用于生成布尔值的包装对象实例。
Boolean 对象除了可以作为构造函数，还可以单独使用，将任意值转为布尔值。这时 Boolean 就是一个单纯的工具方法。

# Number

Number 对象是数值对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用。

## 实例方法

`toFixed`方法先将一个数转为指定位数的小数，然后返回这个小数对应的字符串。

```JS
(10).toFixed(2) // "10.00"
10.005.toFixed(2) // "10.01"
```

# String

`String` 对象是 JavaScript 原生提供的三个包装对象之一，用来生成字符串对象。

## 实例属性

字符串实例的`length`属性返回字符串的长度

## 实例方法

- `charAt`方法返回指定位置的字符串，可以使用下标代替
- `concat`方法连接两个字符串
- `slice`方法从字符串中取出子字符串并返回
- `substring`方法从字符串中取出子字符串并返回，区别在于
  - `substring`方法会**将负值的参数转为 0**进行调用
  - 如果第一个参数大于第二个参数，`substring` 方法会自动更换两个参数的位置。
- `substr`方法从字符串中取出子字符串并返回，区别在于
  - 如果第二个参数是负数，将被自动转为 0
- `indexOf`方法确认参数在字符串中第一次出现的位置，不匹配返回`-1`
- `trim`方法去除字符串两端的空格
- `toLowerCase`/`toUpperCase`方法将字符串转为小写/大写
- `match`方法用于确认字符串是否匹配某个字符串，返回一个数组，成员是匹配的第一个字符串，没有匹配到返回`null`，还可以使用正则来做参数
- `search`方法使用方式同`match`，返回匹配的第一个位置，没有匹配则返回`-1`，可以使用正则来做参数
- `replace`方法用于替换子字符串，一般情况只替换第一个，使用全局匹配的正则时可替换多个
- `split`按照参数分隔字符串为数组

# Math

## 静态方法

- `abs`取绝对值
- `ceil`向上取整
- `floor`向下取整
- `max`取最大值
- `min`取最小值
- `round`四舍五入
- `random`取随机数

# Date

`Date`作为普通函数调用时，返回一个代表当前时间的字符串

## 构造函数

使用`new`命令，返回一个`Date`对象的实例，不传入参数，这个实例就代表当前的时间
可以接受各种格式的参数，返回该参数对应的时间实例

## 静态方法

- `Date.now`方法返回当前时间距离时间零点（1970 年 1 月 1 日 00:00:00 UTC）的毫秒数
- `Date.parse`方法用来解析日期字符串，返回该时间距离时间零点（1970 年 1 月 1 日 00:00:00）的毫秒数，解析失败返回`NaN`

## 实例方法

- `getTime`：返回实例距离 1970 年 1 月 1 日 00:00:00 的毫秒数，等同于 valueOf 方法。
- `getDate`：返回实例对象对应每个月的几号（从 1 开始）。
- `getDay`：返回星期几，星期日为 0，星期一为 1，以此类推。
- `getFullYear`：返回四位的年份。
- `getMonth`：返回月份（0 表示 1 月，11 表示 12 月）。
- `getHours`：返回小时（0-23）。
- `getMilliseconds`：返回毫秒（0-999）。
- `getMinutes`：返回分钟（0-59）。`
- `getSeconds`：返回秒（0-59）。

`Date` 对象提供了一系列`set*`方法，用来设置实例对象的各个方面。除了`getDay`，都有对应的`set*`方法

# JSON

`JSON` 对象是 JavaScript 的原生对象，用来处理 `JSON` 格式数据

## 静态方法

- `JSON.stringify`方法将一个值转为 JSON 字符串
- `JSON.parse`方法将 JSON 字符串转为对应的 JS 值，如果是非规范的 JSON，会抛出错误
