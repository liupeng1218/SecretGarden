<!-- TOC -->

- [new](#new)
- [bin](#bin)
- [call](#call)
- [apply](#apply)

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

# bin

1. 普通函数，绑定 this 指向
2. 构造函数，保证函数原型对象属性不丢失

```JS
Function.prototype.bind = function (context, ...args) {
    if (typeof this !== "function") {
      throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var self = this;

    var fbound = function () {
        self.apply(this instanceof self ? 
            this : 
            context, args.concat(Array.prototype.slice.call(arguments)));
    }

    fbound.prototype = Object.create(self.prototype);

    return fbound;
}

```

# call

# apply
