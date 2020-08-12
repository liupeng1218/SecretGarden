<!-- TOC -->

- [TypeScript](#typescript)
- [特性](#特性)
  - [TypeScript 增加了代码的可读性和可维护性](#typescript-增加了代码的可读性和可维护性)
  - [TypeScript 非常包容](#typescript-非常包容)
  - [TypeScript 拥有活跃的社区](#typescript-拥有活跃的社区)
  - [缺陷](#缺陷)
- [基础](#基础)
  - [原始数据类型](#原始数据类型)
      - [布尔值](#布尔值)
      - [数值](#数值)
      - [字符串](#字符串)
      - [空值](#空值)
      - [null&undefined](#nullundefined)
  - [任意值](#任意值)
  - [类型推论](#类型推论)
  - [联合类型](#联合类型)
  - [对象的类型（接口）](#对象的类型接口)
      - [可选属性](#可选属性)
      - [任意属性](#任意属性)
      - [只读属性](#只读属性)
  - [数组的类型](#数组的类型)
      - [接口标识数组](#接口标识数组)
      - [类数组](#类数组)
  - [函数](#函数)
      - [函数声明](#函数声明)
      - [函数表达式](#函数表达式)
      - [接口定义函数](#接口定义函数)
      - [可选参数](#可选参数)
      - [参数默认值](#参数默认值)
      - [剩余参数](#剩余参数)
      - [重载](#重载)
  - [类型断言](#类型断言)
      - [使用](#使用)
        - [将联合类型断言为其中一个类型](#将联合类型断言为其中一个类型)
        - [将父类断言为具体子类](#将父类断言为具体子类)
        - [将任何一个类型断言为 any](#将任何一个类型断言为-any)
        - [将 any 断言为一个具体的类型](#将-any-断言为一个具体的类型)
      - [限制](#限制)
  - [内置对象](#内置对象)
      - [ECMAScript 的内置对象](#ecmascript-的内置对象)
      - [DOM 和 BOM 的内置对象](#dom-和-bom-的内置对象)
      - [TypeScript 核心库的定义文件](#typescript-核心库的定义文件)
- [进阶](#进阶)
  - [类型别名](#类型别名)
  - [字符串字面量类型](#字符串字面量类型)
  - [元祖](#元祖)
      - [越界元素](#越界元素)
  - [枚举](#枚举)
      - [手动赋值](#手动赋值)
      - [常数项和计算所得项](#常数项和计算所得项)
      - [常数枚举](#常数枚举)
      - [外部枚举](#外部枚举)
  - [类](#类)
      - [readonly](#readonly)
      - [抽象类](#抽象类)
  - [类与接口](#类与接口)
      - [类实现接口](#类实现接口)
      - [接口继承接口](#接口继承接口)
      - [接口继承类](#接口继承类)
  - [泛型](#泛型)
      - [多类型参数](#多类型参数)
      - [泛型约束](#泛型约束)
      - [泛型接口](#泛型接口)
      - [泛型类](#泛型类)
      - [泛型参数的默认类型](#泛型参数的默认类型)
  - [声明合并](#声明合并)
      - [函数合并](#函数合并)
      - [接口的合并](#接口的合并)
      - [类的合并](#类的合并)

<!-- /TOC -->

# TypeScript

`TypeScript` 是 `JavaScript` 的一个超集，主要提供了类型系统和对 `ES6` 的支持，它由 `Microsoft` 开发，代码开源于 `GitHub` 上。

# 特性

## TypeScript 增加了代码的可读性和可维护性

1. 类型系统实际上是最好的文档，大部分的函数看看类型的定义就可以知道如何使用了
2. 可以在编译阶段就发现大部分错误，这总比在运行时候出错好
3. 增强了编辑器和 IDE 的功能，包括代码补全、接口提示、跳转到定义、代码重构等

## TypeScript 非常包容

1. `TypeScript` 是 `JavaScript` 的超集，`.js` 文件可以直接重命名为 `.ts` 即可
2. 即使不显式的定义类型，也能够自动做出类型推论
3. `TypeScript` 的类型系统是图灵完备的，可以定义从简单到复杂的几乎一切类型
4. 即使 `TypeScript` 编译报错，也可以生成 `JavaScript` 文件
5. 兼容第三方库，即使第三方库不是用 `TypeScript` 写的，也可以编写单独的类型文件供 `TypeScript` 读取

## TypeScript 拥有活跃的社区

1. 大部分第三方库都有提供给 `TypeScript` 的类型定义文件
2. `Angular`、`Vue`、`VS Code`、`Ant Design` 等等耳熟能详的项目都是使用 `TypeScript` 编写的
3. `TypeScript` 拥抱了 `ES6` 规范，支持 `ESNext` 草案中处于第三阶状态（Stage 3）的特性

## 缺陷

- 有一定的学习成本，需要理解接口（Interfaces）、泛型（Generics）、类（Classes）、枚举类型（Enums）等前端工程师可能不是很熟悉的概念
- 短期可能会增加一些开发成本，毕竟要多写一些类型的定义，不过对于一个需要长期维护的项目，TypeScript 能够减少其维护成本
- 集成到构建流程需要一些工作量
- 可能和一些库结合的不是很完美

# 基础

## 原始数据类型

在 `TypeScript` 中，使用`:`指定变量的类型，`:`的前后有没有空格都可以。

#### 布尔值

```TS
let isDone: boolean = false;
```

#### 数值

```TS
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

#### 字符串

```TS
let myName: string = 'Tom';
let myAge: number = 25;

// 模板字符串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
```

**注意，使用基础类型的构造函数创造的对象不是基础类型**

#### 空值

用 `void` 表示没有任何返回值的函数

```TS
function alertName(): void {
    alert('My name is Tom');
}
```

声明一个 `void` 类型的变量没有什么用，只能将它赋值为 `undefined` 和 `null`

```TS
let unusable: void = undefined;

```

#### null&undefined

可以使用 `null` 和 `undefined` 来定义这两个原始数据类型  
与 `void` 的区别是，**`undefined` 和 `null` 是所有类型的子类型**。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量

```TS
let u: undefined;
let num: number = u;
```

## 任意值

如果是一个普通类型，在赋值过程中改变类型是不被允许的  
任意值 `any` 类型：

- 可以被赋值为任意类型
- 访问任何属性
- 调用任何方法

声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值

```TS
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
// 在任意值上访问任何属性都是允许的：
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
// 也允许调用任何方法：
let anyThing: any = 'Tom';
anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');
```

**变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型**

## 类型推论

如果没有明确的指定类型，那么 `TypeScript` 会依照类型推论（`Type Inference`）的规则推断出一个类型。

```TS
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'

// 等价于：

let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string
```

**如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查**

## 联合类型

联合类型（`Union Types`）表示取值可以为多种类型中的一种

```TS
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

当 `TypeScript` 不确定一个联合类型的变量到底是哪个类型的时候，**只能访问此联合类型的所有类型里共有的属性或方法**

```TS
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型

```TS
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); // 5
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 编译时报错

// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
```

## 对象的类型（接口）

在 `TypeScript` 中，我们使用接口（`Interfaces`）来定义对象的类型。

在面向对象语言中，接口（`Interfaces`）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（`classes`）去实现（`implement`）。

`TypeScript` 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（`Shape`）」进行描述。

```TS
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
// 定义了一个接口 Person，接着定义了一个变量 tom，它的类型是 Person。这样，我们就约束了 tom 的形状必须和接口 Person 一致。
```

**赋值的时候，变量的形状必须和接口的形状保持一致，缺失属性或多余属性都不可以**

#### 可选属性

希望不要完全匹配一个形状，那么可以用可选属性：

```TS
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```

**可选属性的含义是该属性可以不存在。这时仍然不允许添加未定义的属性**

#### 任意属性

一个接口允许有任意的属性，使用 `[propName: string]` 定义了任意属性的 `key` 取 `string` 类型的值。

```TS
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```

**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**

```TS
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```

一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型

#### 只读属性

对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性

```TS
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};

tom.id = 9527;

// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

**只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**

## 数组的类型

使用「类型 + 方括号」来表示数组

```TS
let fibonacci: number[] = [1, 1, 2, 3, 5];

// 数组的项中不允许出现其他的类型：
let fibonacci: number[] = [1, '1', 2, 3, 5];

// Type 'string' is not assignable to type 'number'.

// 数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：
let fibonacci: number[] = [1, 1, 2, 3, 5];
fibonacci.push('8');

// Argument of type '"8"' is not assignable to parameter of type 'number'.
```

#### 接口标识数组

可以用接口来表述数组

```TS
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
// NumberArray 表示：只要索引的类型是数字时，那么值的类型必须是数字。

```

虽然接口也可以用来描述数组，但是一般使用 `[]` 的简单方法来表示。接口通常用来表示类数组

#### 类数组

类数组（`Array-like Object`）不是数组类型，比如 `arguments`

```TS
function sum() {
    let args: number[] = arguments;
}

// Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more.
```

arguments 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口

```TS
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}
// 们除了约束当索引的类型是数字时，值的类型必须是数字之外，也约束了它还有 length 和 callee 两个属性。
```

`TypeScript`对常用的类数组都有自己的接口定义，如 `IArguments`, `NodeList`, `HTMLCollection` 等：

```TS
function sum() {
    let args: IArguments = arguments;
}
// 其中 IArguments 是 TypeScript 中定义好了的类型，它实际上就是：

interface IArguments {
    [index: number]: any;
    length: number;
    callee: Function;
}
```

## 函数

#### 函数声明

一个函数有输入和输出，要在 `TypeScript` 中对其进行约束，需要把输入和输出都考虑到

```TS

function sum(x: number, y: number): number {
    return x + y;
}
```

**不允许输入多余的（或者少于要求的）参数**

#### 函数表达式

函数表达式中需要手动为接收匿名函数的变量进行定义

```TS
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

#### 接口定义函数

采用函数表达式|接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变。

```TS
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

#### 可选参数

用 `?` 表示可选的参数，**可选参数必须接在必需参数后面**

```TS
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

#### 参数默认值

`TypeScript` 会将**添加了默认值的参数识别为可选参数**，此时就不受**可选参数必须接在必需参数后**的限制了

```TS
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```

#### 剩余参数

`ES6` 中，可以使用 `...items` 的方式获取函数中的剩余参数（`items` 参数）

`items` 是一个数组。可以用数组的类型来定义它

```TS
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

#### 重载

重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。

```TS
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

## 类型断言

类型断言（`Type Assertion`）可以用来手动指定一个值的类型。  
`值 as 类型`

#### 使用

- 联合类型可以被断言为其中一个类型
- 父类可以被断言为子类
- 任何类型都可以被断言为 `any`
- `any` 可以被断言为任何类型

##### 将联合类型断言为其中一个类型

当 `TypeScript` 不确定一个联合类型的变量到底是哪个类型的时候，我们`只能访问此联合类型的所有类型中共有的属性或方法`

```TS
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

类型断言只能够「欺骗」`TypeScript` 编译器，无法避免运行时的错误，反而滥用类型断言可能会导致运行时错误。使用类型断言要格外小心，避免断言后调用方法或引用深层属性

##### 将父类断言为具体子类

当类之间有继承关系时，类型断言也是很常见的：

```TS
class ApiError extends Error {
    code: number = 0;
}
class HttpError extends Error {
    statusCode: number = 200;
}

function isApiError(error: Error) {
    if (typeof (error as ApiError).code === 'number') {
        return true;
    }
    return false;
}
```

##### 将任何一个类型断言为 any

理想情况下，`TypeScript` 的类型系统运转良好，每个值的类型都具体而精确。

有的时候，我们非常确定这段代码不会出错，此时我们可以使用 `as any` 临时断言为 `any` 类型：

```TS
(window as any).foo = 1;
```

##### 将 any 断言为一个具体的类型

在开发中，我们不可避免的需要处理 `any` 类型的变量，它们可能是由于第三方库未能定义好自己的类型，也有可能是历史遗留的或其他人编写的烂代码，还可能是受到 `TypeScript` 类型系统的限制而无法精确定义类型的场景。可以通过类型断言及时的把 any 断言为精确的类型

```TS
function getCacheData(key: string): any {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom = getCacheData('tom') as Cat;
tom.run();
```

#### 限制

并不是任何一个类型都可以被断言为任何另一个类型。要使得 A 能够被断言为 B，只需要 A 兼容 B 或 B 兼容 A 即可，这也是为了在类型断言时的安全考虑，毕竟毫无根据的断言是非常危险的。

## 内置对象

`JavaScript` 中有很多内置对象，它们可以直接在 `TypeScript` 中当做定义好了的类型。

内置对象是指根据标准在全局作用域（`Global`）上存在的对象。这里的标准是指 `ECMAScript` 和其他环境（比如 `DOM`）的标准。

#### ECMAScript 的内置对象

`ECMAScript` 标准提供的内置对象有：

`Boolean`、`Error`、`Date`、`RegExp` 等。

我们可以在 `TypeScript` 中将变量定义为这些类型：

```TS
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```

#### DOM 和 BOM 的内置对象

`DOM` 和 `BOM` 提供的内置对象有：

`Document`、`HTMLElement`、`Event`、`NodeList`等。

`TypeScript` 中会经常用到这些类型：

```TS
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```

#### TypeScript 核心库的定义文件

`TypeScript` 核心库的定义文件中定义了所有浏览器环境需要用到的类型，并且是预置在 `TypeScript` 中的。

当你在使用一些常用的方法的时候，`TypeScript` 实际上已经帮你做了很多类型判断的工作了，比如：

```TS
Math.pow(10, '2');

// index.ts(1,14): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

# 进阶

## 类型别名

使用 `type` 创建类型别名。类型别名常用于联合类型。

```TS
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

## 字符串字面量类型

使用 `type` 进行定义字符串字面量类型，用来约束**取值只能是某几个字符串中的一个**。

```TS
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dblclick'); // 报错，event 不能为 'dblclick'

// index.ts(7,47): error TS2345: Argument of type '"dblclick"' is not assignable to parameter of type 'EventNames'.
```

## 元祖

数组合并了相同类型的对象，而元组（`Tuple`）合并了不同类型的对象。

```TS
// 定义一对值分别为 string 和 number 的元组：
let tom: [string, number] = ['Tom', 25];
```

当直接对元组类型的变量进行初始化或者赋值的时候，**需要提供所有元组类型中指定的项**

#### 越界元素

当添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型：

```TS
let tom: [string, number];
tom = ['Tom', 25];
tom.push('male');
tom.push(true);

// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

## 枚举

枚举（`Enum`）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。

枚举使用 `enum` 关键字来定义：

```TS
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```

枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射：

```TS

enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
```

#### 手动赋值

也可以给枚举项手动赋值，未手动赋值的枚举项会接着上一个枚举项递增

```TS
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 3); // true
console.log(Days["Wed"] === 3); // true
console.log(Days[3] === "Sun"); // false
console.log(Days[3] === "Wed"); // true
```

手动赋值的枚举项可以不是数字，此时需要使用类型断言来让 `tsc` 无视类型检查 (编译出的 js 仍然是可用的)：

```TS
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
```

#### 常数项和计算所得项

枚举项有两种类型：常数项（`constant member`）和计算所得项（`computed member`）。

```TS
enum Color {Red, Green, Blue = "blue".length};
```

如果紧接在计算所得项后面的是未手动赋值的项，那么它就会因为**无法获得初始值而报错**：

```TS
enum Color {Red = "red".length, Green, Blue};

// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

#### 常数枚举

常数枚举是使用 `const enum`定义的枚举类型：

```TS
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

常数枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员。**常数枚举不能包含计算所得项**。

#### 外部枚举

外部枚举（`Ambient Enums`）是使用 `declare enum` 定义的枚举类型：

```TS
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

`declare` 定义的类型只会用于编译时的检查，编译结果中会被删除。

## 类

`TypeScript`中的类在`ES6`，`ES7`的基础上添加了三种修饰符

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问
- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的

#### readonly

`readonly`只读属性关键字，只允许出现在属性声明或索引签名或构造函数中。如果 `readonly` 和其他访问修饰符**同时存在的话需要写在其后面**。

```TS
class Animal {
  readonly name;
  public constructor(name) {
    this.name = name;
  }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';

// index.ts(10,3): TS2540: Cannot assign to 'name' because it is a read-only property.
```

#### 抽象类

`abstract` 用于定义抽象类和其中的抽象方法。

抽象类是不允许被实例化

```TS
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

let a = new Animal('Jack');

// index.ts(9,11): error TS2511: Cannot create an instance of the abstract class 'Animal'.
```

抽象类中的抽象方法必须被子类实现

```TS
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

class Cat extends Animal {
  public eat() {
    console.log(`${this.name} is eating.`);
  }
}

let cat = new Cat('Tom');

// index.ts(9,7): error TS2515: Non-abstract class 'Cat' does not implement inherited abstract member 'sayHi' from class 'Animal'.
```

## 类与接口

接口可以对类的部分行为进行抽象

#### 类实现接口

一个类只能继承自另一个类，有时候不同类之间可以有一些共有的特性，这时候就可以把特性提取成接口（`interfaces`），用 `implements` 关键字来实现。这个特性大大提高了面向对象的灵活性。

```TS
/*
门是一个类，防盗门是门的子类。
如果防盗门有一个报警器的功能，我们可以简单的给防盗门添加一个报警方法。
这时候如果有另一个类，车，也有报警器的功能，就可以考虑把报警器提取出来，作为一个接口，防盗门和车都去实现它
*/
interface Alarm {
    alert(): void;
}

class Door {
}

class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert');
    }
}

class Car implements Alarm {
    alert() {
        console.log('Car alert');
    }
}
```

#### 接口继承接口

接口与接口之间可以是继承关系：

```TS
interface Alarm {
    alert(): void;
}

interface LightableAlarm extends Alarm {
    lightOn(): void;
    lightOff(): void;
}
// LightableAlarm 继承了 Alarm，除了拥有 alert 方法之外，还拥有两个新方法 lightOn 和 lightOff。
```

#### 接口继承类

在`TypeScript`中可以使用类继承接口

```TS
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

## 泛型

泛型（`Generics`）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```TS
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']
```

函数名后添加 `<T>`，其中 `T` 用来指代任意输入的类型，在后面的输入 `value: T` 和输出 `Array<T>` 中即可使用了。

#### 多类型参数

定义泛型的时候，可以一次定义多个类型参数：

```TS
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```

#### 泛型约束

在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法。

可以对泛型进行约束，只允许这个函数传入那些包含所需属性的变量。这就是泛型约束：

```TS
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

#### 泛型接口

可以使用含有泛型的接口来定义函数的形状：

```TS
interface CreateArrayFunc {
    <T>(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

#### 泛型类

泛型也可以用于类的类型定义中

```TS
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

#### 泛型参数的默认类型

可以为泛型中的类型参数指定默认类型。当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推测出时，这个默认类型就会起作用。

```TS
function createArray<T = string>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
```

## 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型：

#### 函数合并

使用重载定义多个函数类型：

```TS
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

#### 接口的合并

接口中的属性在合并时会简单的合并到一个接口中：

```TS
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```

**合并的属性的类型必须是唯一的**

#### 类的合并

类的合并与接口的合并规则一致。
