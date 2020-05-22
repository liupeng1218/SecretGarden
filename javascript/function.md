> [简明 JavaScript 函数式编程](https://juejin.im/post/5d70e25de51d453c11684cc4#heading-1) > [JavaScript 专题之函数柯里化](https://github.com/mqyqingfeng/Blog/issues/42)

# 什么是

函数式编程的思维过程是完全不同的，它的着眼点是函数，而不是过程，它强调的是如何通过函数的组合变换去解决问题，而不是我通过写什么样的语句去解决问题，当你的代码越来越多的时候，这种函数的拆分和组合就会产生出强大的力量。

# 为什么

函数实际上是一个关系，或者说是一种映射，而这种映射关系是可以组合的，一旦我们知道一个函数的输出类型可以匹配另一个函数的输入，那他们就可以进行组合
在我们的编程世界中，我们需要处理的其实也只有“数据”和“关系”，而关系就是函数。我们所谓的编程工作也不过就是在找一种映射关系，一旦关系找到了，问题就解决了，剩下的事情，就是让数据流过这种关系，然后转换成另一个数据罢了。

# 特点

## 函数是“一等公民”

这是函数式编程得以实现的前提，因为我们基本的操作都是在操作函数。这个特性意味着函数与其他数据类型一样，处于平等地位，可以赋值给其他变量，也可以作为参数，传入另一个函数，或者作为别的函数的返回值

## 声明式编程

函数式编程大多时候都是在声明我需要做什么，而非怎么去做。这种编程风格称为 声明式编程 。这样有个好处是代码的可读性特别高，因为声明式代码大多都是接近自然语言的，同时，它解放了大量的人力，因为它不关心具体的实现，因此它可以把优化能力交给具体的实现，这也方便我们进行分工协作。

## 惰性执行

所谓惰性执行指的是函数只在需要的时候执行，即不产生无意义的中间变量

## 无状态和数据不可变

- 数据不可变： 它要求你所有的数据都是不可变的，这意味着如果你想修改一个对象，那你应该创建一个新的对象用来修改，而不是修改已有的对象。
- 无状态： 主要是强调对于一个函数，不管你何时运行，它都应该像第一次运行一样，给定相同的输入，给出相同的输出，完全不依赖外部状态的变化。

## 没有副作用

在完成函数主要功能之外完成的其他副要功能。在我们函数中最主要的功能当然是根据输入返回结果，而在函数中我们最常见的副作用就是随意操纵外部变量

## 纯函数

- 不依赖外部状态（无状态）： 函数的的运行结果不依赖全局变量，this 指针，IO 操作等。
- 没有副作用（数据不变）： 不修改全局变量，不修改入参。

# 流水线

如果说函数式编程中有两种操作是必不可少的那无疑就是柯里化（Currying）和函数组合（Compose），柯里化其实就是流水线上的加工站，函数组合就是我们的流水线，它由多个加工站组成。

## 加工站-柯里化

柯里化的意思是将一个多元函数，转换成一个依次调用的单元函数。

### 部分函数应用 vs 柯里化

经常有人搞不清柯里化和部分函数应用 ( Partial Function Application )，经常把他们混为一谈，其实这是不对的，在维基百科里有明确的定义，部分函数应用强调的是固定一定的参数，返回一个更小元的函数。通过以下表达式展示出来就明显了：

```JS
// 柯里化
f(a,b,c) → f(a)(b)(c)
// 部分函数调用
f(a,b,c) → f(a)(b,c) / f(a,b)(c)
```

柯里化强调的是生成单元函数，部分函数应用的强调的固定任意元参数，而我们平时生活中常用的其实是部分函数应用，这样的好处是可以固定参数，降低函数通用性，提高函数的适合用性。

### 高级柯里化

现成的库大多都提供了 `curry` 函数的实现，但是我们使用的 `Lodash` ，`Ramda` 这些库中实现的 `curry` 函数的行为和柯里化不太一样,这些库中的 `curry` 函数都做了很多优化，导致这些库中实现的柯里化其实不是纯粹的柯里化，我们可以把他们理解为“高级柯里化”。这些版本实现可以根据你输入的参数个数，返回一个柯里化函数/结果值。即，如果你给的参数个数满足了函数条件，则返回值。这样可以解决一个问题，就是如果一个函数是多输入，就可以避免使用 `(a)(b)(c)` 这种形式传参了

### 柯里化的应用

通常，我们在实践中使用柯里化都是为了把某个函数变得单值化，这样可以增加函数的多样性，使得其适用性更强：

```js
const replace = curry((a, b, str) => str.replace(a, b))
const replaceSpaceWith = replace(/\s*/)
const replaceSpaceWithComma = replaceSpaceWith(',')
const replaceSpaceWithDash = replaceSpaceWith('-')
```

通过上面这种方式，我们从一个 replace 函数中产生很多新函数，可以在各种场合进行使用。

## 流水线-函数组合

借助 `curry`，已经可以很轻松的构造一个加工站了，现在就是我们组合成流水线的时候了。

### 函数组合

函数组合的目的是将多个函数组合成一个函数。下面来看一个简化版的实现：

```js
const compose = (f, g) => (x) => f(g(x))

const f = (x) => x + 1
const g = (x) => x * 2
const fg = compose(f, g)
fg(1) //3
```

我们可以看到 `compose` 就实现了一个简单的功能：形成了一个全新的函数，而这个函数就是一条从 `g -> f` 的流水线。同时我们可以很轻易的发现 `compose` 其实是满足结合律的

```JS
compose(f, compose(g, t)) = compose(compose(f, g), t)  = f(g(t(x)))

```

只要其顺序一致，最后的结果是一致的，因此，我们可以写个更高级的 `compose`，支持多个函数组合：

```JS
const compose = (...fns) => (...args) => fns.reduceRight((val, fn) => fn.apply(null, [].concat(val)), args);

const f = x => x + 1;
const g = x => x * 2;
const t = (x, y) => x + y;

let fgt = compose(f, g, t);
fgt(1, 2); // 3 -> 6 -> 7

```

### 函数组合应用

考虑一个小功能：将数组最后一个元素大写，假设 `log`, `head`，`reverse`，`toUpperCase` 函数存在（我们通过 `curry` 可以很容易写出来）
命令式的写法：

```JS
log(toUpperCase(head(reverse(arr))))
```

面向对象的写法：

```JS
arr.reverse()
  .head()
  .toUpperCase()
  .log()
```

链式调用看起来顺眼多了，然而问题在于，原型链上可供我们链式调用的函数是有限的，而需求是无限的 ，这限制了我们的逻辑表现力。
再看看，现在通过组合，我们如何实现之前的功能：

```JS
const upperLastItem = compose(log, toUpperCase, head, reverse);
```

通过参数我们可以很清晰的看出发生了 `uppderLastItem` 做了什么，它完成了一套流水线，所有经过这条流水线的参数都会经历：`reverse -> head -> toUpperCase -> log` 这些函数的加工，最后生成结果。

### 函数组合的好处

函数组合的好处显而易见，它让代码变得简单而富有可读性，同时通过不同的组合方式，我们可以轻易组合出其他常用函数，让我们的代码更具表现力

```JS
// 组合方式 1
const last = compose(head, reverse);
const shout = compose(log, toUpperCase);
const shoutLast = compose(shout, last);
// 组合方式 2
const lastUppder = compose(toUpperCase, head, reverse);
const logLastUpper = compose(log, lastUppder);
```

### 实践经验

在使用柯里化和函数组合的时候，有一些经验可以借鉴一下：

#### 柯里化中把要操作的数据放到最后

因为我们的输出通常是需要操作的数据，这样当我们固定了之前的参数（我们可以称为配置）后，可以变成一个单元函数，直接被函数组合使用，这也是其他的函数式语言遵循的规范：

```JS
const split = curry((x, str) => str.split(x));
const join = curry((x, arr) => arr.join(x));
const replaceSpaceWithComma = compose(join(','), split(' '));
const replaceCommaWithDash = compose(join('-'), split(','));

```

#### 函数组合中函数要求单输入

函数组合有个使用要点，就是中间的函数一定是单输入的，这个很好理解，之前也说过了，因为函数的输出都是单个的（数组也只是一个元素）。

#### 多参考 Ramda

现有的函数式编程工具库很多，`Lodash/fp` 也提供了，但是不是很推荐使用 `Lodash/fp` 的函数库，因为它的很多函数把需要处理的参数放在了首位（ 例如 map ）这不符合我们之前说的最佳实践。
这里推荐使用 `Ramda`，它应该是目前最符合函数式编程的工具库，它里面的所有函数都是 `curry` 的，而且需要操作的参数都是放在最后的。上述的 `split`，`join`，`replace` 这些基本的都在 `Ramda` 中可以直接使用，它一共提供了 200 多个超实用的函数，合理使用可以大大提高你的编程效率（目前我的个人经验来说，我需要的功能它 90%都提供了）。

# 总结

前面介绍了很多函数式编程的概念可以总结出函数式编程的优点：

- 代码简洁，开发快速：函数式编程大量使用函数的组合，函数的复用率很高，减少了代码的重复，因此程序比较短，开发速度较快。Paul Graham 在《黑客与画家》一书中写道：同样功能的程序，极端情况下，Lisp 代码的长度可能是 C 代码的二十分之一。
- 接近自然语言，易于理解：函数式编程大量使用声明式代码，基本都是接近自然语言的，加上它没有乱七八糟的循环，判断的嵌套，因此特别易于理解。
- 易于"并发编程"：函数式编程没有副作用，所以函数式编程不需要考虑“死锁”（Deadlock），所以根本不存在“锁”线程的问题。
- 更少的出错概率：因为每个函数都很小，而且相同输入永远可以得到相同的输出，因此测试很简单，同时函数式编程强调使用纯函数，没有副作用，因此也很少出现奇怪的 Bug。

因此，如果用一句话来形容函数式编程，应该是：`Less code, fewer bugs` 。因为写的代码越少，出错的概率就越小。人是最不可靠的，我们应该尽量把工作交给计算机。

一眼看下来好像函数式可以解决所有的问题，但是实际上，函数式编程也不是什么万能的灵丹妙药。正因为函数式编程有以上特点，所以它天生就有以下缺陷：

- 性能：函数式编程相对于指令式编程，性能绝对是一个短板，因为它往往会对一个方法进行过度包装，从而产生上下文切换的性能开销。同时，在 JS 这种非函数式语言中，函数式的方式必然会比直接写语句指令慢（引擎会针对很多指令做特别优化）。就拿原生方法 map 来说，它就要比纯循环语句实现迭代慢 8 倍。
- 资源占用：在 JS 中为了实现对象状态的不可变，往往会创建新的对象，因此，它对垃圾回收（Garbage Collection）所产生的压力远远超过其他编程方式。这在某些场合会产生十分严重的问题。
- 递归陷阱：在函数式编程中，为了实现迭代，通常会采用递归操作，为了减少递归的性能开销，我们往往会把递归写成尾递归形式，以便让解析器进行优化。但是众所周知，JS 是不支持尾递归优化的（虽然 ES6 中将尾递归优化作为了一个规范，但是真正实现的少之又少，传送门）

因此，在性能要求很严格的场合，函数式编程其实并不是太合适的选择。
但是换种思路想，软件工程界从来就没有停止过所谓的银弹之争，却也从来没诞生过什么真正的银弹，各种编程语言层出不穷，各种框架日新月异，各种编程范式推陈出新，结果谁也没有真正的替代谁。
学习函数式编程真正的意义在于：让你意识到在指令式编程，面向对象编程之外，还有一种全新的编程思路，一种用函数的角度去抽象问题的思路。学习函数式编程能大大丰富你的武器库，不然，当你手中只有一个锤子，你看什么都像钉子。

我们完全可以在日常工作中将函数式编程作为一种辅助手段，在条件允许的前提下，借鉴函数式编程中的思路，例如：

- 多使用纯函数减少副作用的影响。
- 使用柯里化增加函数适用率。

# 柯里化

## 定义

维基百科中对柯里化 (Currying) 的定义为：

> In mathematics and computer science, currying is the technique of translating the evaluation of a function that takes multiple arguments (or a tuple of arguments) into evaluating a sequence of functions, each with a single argument.

翻译成中文：

> **在数学和计算机科学中，柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术**。

举个例子：

```JS
function add(a, b) {
return a + b;
}

// 执行 add 函数，一次传入两个参数即可
add(1, 2) // 3

// 假设有一个 curry 函数可以做到柯里化
var addCurry = curry(add);
addCurry(1)(2) // 3
```

## 用途

我们会讲到如何写出这个 `curry` 函数，并且会将这个 `curry` 函数写的很强大，但是在编写之前，我们需要知道柯里化到底有什么用？

举个例子：

```JS
// 示意而已
function ajax(type, url, data) {
    var xhr = new XMLHttpRequest();
    xhr.open(type, url, true);
    xhr.send(data);
}

// 虽然 ajax 这个函数非常通用，但在重复调用的时候参数冗余
ajax('POST', 'www.test.com', "name=kevin")
ajax('POST', 'www.test2.com', "name=kevin")
ajax('POST', 'www.test3.com', "name=kevin")

// 利用 curry
var ajaxCurry = curry(ajax);

// 以 POST 类型请求数据
var post = ajaxCurry('POST');
post('www.test.com', "name=kevin");

// 以 POST 类型请求来自于 www.test.com 的数据
var postFromTest = post('www.test.com');
postFromTest("name=kevin");
```

想想 `jQuery` 虽然有 `$.ajax` 这样通用的方法，但是也有 `$.get` 和 `$.post` 的语法糖。

`curry` 的这种用途可以理解为：参数复用。本质上是降低通用性，提高适用性。

可是即便如此，是不是依然感觉没什么用呢？

如果我们仅仅是把参数一个一个传进去，意义可能不大，但是如果我们是把柯里化后的函数传给其他函数比如 `map` 呢？

举个例子：

比如我们有这样一段数据：

```JS
var person = [{name: 'kevin'}, {name: 'daisy'}]
```

如果我们要获取所有的 name 值，我们可以这样做：

```JS
var name = person.map(function (item) {
    return item.name;
})
```

不过如果我们有 `curry` 函数：

```JS
var prop = curry(function (key, obj) {
    return obj[key]
});

var name = person.map(prop('name'))
```

我们为了获取 `name` 属性还要再编写一个 `prop` 函数，是不是又麻烦了些？

但是要注意，`prop` 函数编写一次后，以后可以多次使用，实际上代码从原本的三行精简成了一行，而且你看代码是不是更加易懂了？

`person.map(prop('name'))` 就好像直白的告诉你：`person` 对象遍历(map)获取(prop) `name` 属性。

# 实现

## 第一版

未来我们会接触到更多有关柯里化的应用，不过那是未来的事情了，现在我们该编写这个 `curry` 函数了。

一个经常会看到的 `curry` 函数的实现为：

```JS
// 第一版
var curry = function (fn) {
    var args = [].slice.call(arguments, 1);
    return function() {
        var newArgs = args.concat([].slice.call(arguments));
        return fn.apply(this, newArgs);
    };
};
// 我们可以这样使用：

function add(a, b) {
    return a + b;
}

var addCurry = curry(add, 1, 2);
addCurry() // 3
//或者
var addCurry = curry(add, 1);
addCurry(2) // 3
//或者
var addCurry = curry(add);
addCurry(1, 2) // 3
```

已经有柯里化的感觉了，但是还没有达到要求，不过我们可以把这个函数用作辅助函数，帮助我们写真正的 curry 函数。

## 第二版

```JS
// 第二版
function sub_curry(fn) {
    var args = [].slice.call(arguments, 1);
    return function() {
        return fn.apply(this, args.concat([].slice.call(arguments)));
    };
}

function curry(fn, length) {

    length = length || fn.length;

    var slice = Array.prototype.slice;

    return function() {
        if (arguments.length < length) {
            var combined = [fn].concat(slice.call(arguments));
            return curry(sub_curry.apply(this, combined), length - arguments.length);
        } else {
            return fn.apply(this, arguments);
        }
    };
}
```

我们验证下这个函数：

```JS
var fn = curry(function(a, b, c) {
    return [a, b, c];
});

fn("a", "b", "c") // ["a", "b", "c"]
fn("a", "b")("c") // ["a", "b", "c"]
fn("a")("b")("c") // ["a", "b", "c"]
fn("a")("b", "c") // ["a", "b", "c"]
```
