<!-- TOC -->

- [类数组](#类数组)
- [forEach 的 return](#foreach-的-return)
- [JS 判断是否包含值](#js-判断是否包含值)
- [数组扁平化](#数组扁平化)
- [深浅拷贝](#深浅拷贝)
- [数组去重](#数组去重)

<!-- /TOC -->

# 类数组

函数中的`arguments`，使用`getElementsByTagName`获取的`HTMLCollection`，一级使用`querySeleor`获取的`nodeList`都是类数组对象
它们都从 0 开始开裂，有`length`属性，但是不是数组，数组的很多方法也不能调用

**转化**

1. Array.prototype.slice.call

```JS
function sum(a, b) {
  let args = Array.prototype.slice.call(arguments);
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3

```

2. Array.from

```JS
function sum(a, b) {
  let args = Array.from(arguments);
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3

```

3. ES6 展开运算符`...`

```JS
function sum(a, b) {
  let args = [...arguments];
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3

```

4. Array.prototype.concat.apply

```JS
function sum(a, b) {
  let args = Array.prototype.concat.apply([], arguments);//apply方法会把第二个参数展开
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3

```

# forEach 的 return

在`forEach`中使用`return`不会中断循环

**方法**

1. 使用`try`监视，需要中断时抛出异常
2. 使用`every`和`some`方式替换代替`forEach`

# JS 判断是否包含值

1. array.indexOf

```JS
var arr=[1,2,3,4];
var index=arr.indexOf(3);
console.log(index);

```

2. array.includes

```JS
var arr=[1,2,3,4];
if(arr.includes(3))
    console.log("存在");
else
    console.log("不存在");

```

3. array.find

```JS
var arr=[1,2,3,4];
var result = arr.find(item =>{
    return item > 3
});
console.log(result);

```

4. array.findIndex

```JS
var arr=[1,2,3,4];
var result = arr.findIndex(item =>{
    return item > 3
});
console.log(result);

```

# 数组扁平化

数组扁平化即将多维数组转化为一维数组

1. ES6 中的`flat`方法
2. replace + split

```JS
ary = str.replace(/(\[|\])/g, '').split(',')
```

3. replace + JSON.parse

```js
str = str.replace(/(\[|\])/g, '')
str = '[' + str + ']'
ary = JSON.parse(str)
```

4. 递归

```JS
let result = [];
let fn = function(ary) {
  for(let i = 0; i < ary.length; i++) {
    let item = ary[i];
    if (Array.isArray(ary[i])){
      fn(item);
    } else {
      result.push(item);
    }
  }
}

```

5. reduce

```JS
function flatten(ary) {
    return ary.reduce((pre, cur) => {
        return pre.concat(Array.isArray(cur) ? flatten(cur) : cur);
    }, []);
}
let ary = [1, 2, [3, 4], [5, [6, 7]]]
console.log(flatten(ary))

```

6. 扩展运算符

```JS
//只要有一个元素有数组，那么循环继续
while (ary.some(Array.isArray)) {
  ary = [].concat(...ary);
}

```

# 深浅拷贝

# 数组去重