<!-- TOC -->

- [forEach 的 return](#foreach-%e7%9a%84-return)
- [JS 判断是否包含值](#js-%e5%88%a4%e6%96%ad%e6%98%af%e5%90%a6%e5%8c%85%e5%90%ab%e5%80%bc)

<!-- /TOC -->



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
