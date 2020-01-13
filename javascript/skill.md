<!-- TOC -->

- [数组](#数组)
  - [扁平化](#扁平化)
  - [去重](#去重)
  - [排序](#排序)
  - [最值](#最值)
- [类数组转化](#类数组转化)

<!-- /TOC -->

# 数组

## 扁平化

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

## 去重

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

## 排序

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

## 最值

初级
先排序再取值

终极
`Math.max()`和`Math.min()`是 `Math` 对象内置的方法,获取最值

```JS
Math.max(...[1,2,3,4]) //4
Math.max.apply(this,[1,2,3,4]) //4
```

# 类数组转化

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
