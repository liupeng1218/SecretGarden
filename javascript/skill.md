<!-- TOC -->

- [1. 数组](#1-%e6%95%b0%e7%bb%84)
  - [1.1. 扁平化](#11-%e6%89%81%e5%b9%b3%e5%8c%96)
  - [1.2. 去重](#12-%e5%8e%bb%e9%87%8d)
  - [1.3. 排序](#13-%e6%8e%92%e5%ba%8f)
  - [1.4. 最值](#14-%e6%9c%80%e5%80%bc)
- [2. 类数组转化](#2-%e7%b1%bb%e6%95%b0%e7%bb%84%e8%bd%ac%e5%8c%96)
- [3. forEach 的 return](#3-foreach-%e7%9a%84-return)
- [4. JS 判断是否包含值](#4-js-%e5%88%a4%e6%96%ad%e6%98%af%e5%90%a6%e5%8c%85%e5%90%ab%e5%80%bc)

<!-- /TOC -->

# 1. 数组

## 1.1. 扁平化

初级
利用递归和`concat`结构实现逐级扁平

```JS
function flatten(arr) {
    while(arr.some(item=>Array.isArray(item))) {
        arr = [].concat(...arr);
    }
    return arr;
}
flatten([1,[2,3]]) //[1,2,3]
flatten([1,[2,3,[4,5]]) //[1,2,3,4,5]
```

终极

`Array.flat(n)`是 ES10 扁平数组的 api,n 表示维度,n 值为 Infinity 时维度为无限大

```JS
[1,[2,3]].flat(1) //[1,2,3]
[1,[2,3,[4,5]]].flat(2) //[1,2,3,4,5]
[1,[2,3,[4,5]]].toString()  //'1,2,3,4,5'
[1[2,3,[4,5[...]].flat(Infinity) //[1,2,3,4...n]
```

## 1.2. 去重

初级

循环数据判断存在

```JS
Array.prototype.distinct = function() {
    const map = {}
    const result = []
    for (const n of this) {
        if (!(n in map)) {
            map[n] = 1
            result.push(n)
        }
    }
    return result
}
[1,2,3,3,4,4].distinct(); //[1,2,3,4]

```

终极

`Set` 是 `ES6` 新出来的一种一种定义不重复数组的数据类型 `Array.from`是将类数组转化为数组 `...`是扩展运算符,将`Set`里面的值转化为字符串

```JS
Array.from(new Set([1,2,3,3,4,4])) //[1,2,3,4]
[...new Set([1,2,3,3,4,4])] //[1,2,3,4]
```

## 1.3. 排序

初级
冒泡排序

```JS
Array.prototype.bubleSort=function () {
    let arr=this,
        len = arr.length;
    for (let outer = len; outer >= 2; outer--) {
      for (let inner = 0; inner <= outer - 1; inner++) {
        if (arr[inner] > arr[inner + 1]) {
          //升序
          [arr[inner], arr[inner + 1]] = [arr[inner + 1], arr[inner]];
          console.log([arr[inner], arr[inner + 1]]);
        }
      }
    }
    return arr;
  }
[1,2,3,4].bubleSort() //[1,2,3,4]

```

终极
原生`sort`方法排序

```JS
[1,2,3,4].sort((a, b) => a - b); // [1, 2,3,4],默认是升序
[1,2,3,4].sort((a, b) => b - a); // [4,3,2,1] 降序

```

## 1.4. 最值

初级
先排序再取值

终极
`Math.max()`和`Math.min()`是 `Math` 对象内置的方法,获取最值

```JS
Math.max(...[1,2,3,4]) //4
Math.max.apply(this,[1,2,3,4]) //4
```

# 2. 类数组转化

函数中的`arguments`，使用`getElementsByTagName`获取的`HTMLCollection`，一级使用`querySeleor`获取的`nodeList`都是类数组对象
它们都从 0 开始开裂，有`length`属性，但是不是数组，数组的很多方法也不能调用

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

# 3. forEach 的 return

在`forEach`中使用`return`不会中断循环

**方法**

1. 使用`try`监视，需要中断时抛出异常
2. 使用`every`和`some`方式替换代替`forEach`

# 4. JS 判断是否包含值

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
