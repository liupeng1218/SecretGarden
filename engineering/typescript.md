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
