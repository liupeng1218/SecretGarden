<!-- TOC -->

- [new](#new)
- [instanceof](#instanceof)
- [call/apply/bind](#callapplybind)
  - [call](#call)
  - [apply](#apply)
  - [bind](#bind)

<!-- /TOC -->


# new

new 调用实现三个效果
1. 实例可以访问到实例属性
2. 实例可以访问构造函数原型所在原型链上属性
3. 如果返回不是引用类型，则返回原型


```JS
function newOperator(ctor, ...args) {
    if(typeof ctor !== 'function'){
      throw 'newOperator function the first param must be a function';
    }
    let obj = Object.create(ctor.prototype);
    let res = ctor.apply(obj, args);
    
    let isObject = typeof res === 'object' && res !== null;
    let isFunction = typoof res === 'function';
    return isObect || isFunction ? res : obj;
};

```

# instanceof

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
# call/apply/bind


`call` 和 `apply` 都是为了解决改变 `this` 的指向。作用都是相同的，只是传参的方式不同。

除了第一个参数外， `call` 可以接收一个参数列表， `apply` 只接受一个参数数组。

## call
+ 不传入第一个参数，那么默认为 window
+ 改变了 this 指向，让新的对象可以执行该函数。那么思路是否可以变成给新的对象添加一个函数，然后在执行完以后删除？
```JS
Function.prototype.myCall = function (context) {
  var context = context || window
  // 给 context 添加一个属性
  // getValue.call(a, 'yck', '24') => a.fn = getValue
  context.fn = this
  // 将 context 后面的参数取出来
  var args = [...arguments].slice(1)
  // getValue.call(a, 'yck', '24') => a.fn('yck', '24')
  var result = context.fn(...args)
  // 删除 fn
  delete context.fn
  return result
}

```
## apply
```JS
Function.prototype.myApply = function (context) {
  var context = context || window
  context.fn = this

  var result
  // 需要判断是否存储第二个参数
  // 如果存在，就将第二个参数展开
  if (arguments[1]) {
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }

  delete context.fn
  return result
}
```

## bind
`bind` 和其他两个方法作用也是一致的，只是该方法会返回一个函数。并且我们可以通过 `bind` 实现柯里化。
```JS
Function.prototype.myBind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  var _this = this
  var args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}

```

