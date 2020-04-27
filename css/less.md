<!-- TOC -->

- [1. Less](#1-less)
- [2. 变量](#2-%e5%8f%98%e9%87%8f)
  - [2.1. 普通变量](#21-%e6%99%ae%e9%80%9a%e5%8f%98%e9%87%8f)
  - [2.2. 选择器/属性/url 变量](#22-%e9%80%89%e6%8b%a9%e5%99%a8%e5%b1%9e%e6%80%a7url-%e5%8f%98%e9%87%8f)
  - [2.3. 申明变量](#23-%e7%94%b3%e6%98%8e%e5%8f%98%e9%87%8f)
  - [2.4. 变量运算](#24-%e5%8f%98%e9%87%8f%e8%bf%90%e7%ae%97)
  - [2.5. 变量定义变量](#25-%e5%8f%98%e9%87%8f%e5%ae%9a%e4%b9%89%e5%8f%98%e9%87%8f)
  - [2.6. 变量范围](#26-%e5%8f%98%e9%87%8f%e8%8c%83%e5%9b%b4)
- [3. 嵌套](#3-%e5%b5%8c%e5%a5%97)
- [4. 媒体查询](#4-%e5%aa%92%e4%bd%93%e6%9f%a5%e8%af%a2)
- [5. 混合](#5-%e6%b7%b7%e5%90%88)
  - [5.1. 传递参数](#51-%e4%bc%a0%e9%80%92%e5%8f%82%e6%95%b0)
  - [5.2. 不定参](#52-%e4%b8%8d%e5%ae%9a%e5%8f%82)
  - [5.3. 匹配模式](#53-%e5%8c%b9%e9%85%8d%e6%a8%a1%e5%bc%8f)
  - [5.4. 命名空间](#54-%e5%91%bd%e5%90%8d%e7%a9%ba%e9%97%b4)
  - [5.5. 条件语句](#55-%e6%9d%a1%e4%bb%b6%e8%af%ad%e5%8f%a5)
  - [5.6. 条件运算符](#56-%e6%9d%a1%e4%bb%b6%e8%bf%90%e7%ae%97%e7%ac%a6)
  - [5.7. 循环](#57-%e5%be%aa%e7%8e%af)
- [6. 属性拼接](#6-%e5%b1%9e%e6%80%a7%e6%8b%bc%e6%8e%a5)
  - [6.1. 空格拼接](#61-%e7%a9%ba%e6%a0%bc%e6%8b%bc%e6%8e%a5)
  - [6.2. 逗号拼接](#62-%e9%80%97%e5%8f%b7%e6%8b%bc%e6%8e%a5)
- [7. 继承](#7-%e7%bb%a7%e6%89%bf)
- [8. 导入](#8-%e5%af%bc%e5%85%a5)
- [9. 函数](#9-%e5%87%bd%e6%95%b0)

<!-- /TOC -->

# 1. Less

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。

开发时需要进行转义成标准 css 才可使用。

# 2. 变量

## 2.1. 普通变量

以 `@` 开头定义变量，在使用时直接调用即可。 变量重复定义，后定义的值会覆盖前面的变量

```less
@color: #333;
div {
  color: @color;
}
```

## 2.2. 选择器/属性/url 变量

声明选择器/属性/url 变量。在使用的时候需要添加大括号`{}`，选择器变量的前面可以添加选择操作符

**选择器**

```less
@mySelector: #wrap;
@Wrap: wrap;
@{mySelector}{ //变量名 必须使用大括号包裹
  color: #999;
  width: 50%;
}
.@{Wrap}{
  color:#ccc;
}
#@{Wrap}{
  color:#666;
}
/* 生成的 CSS */
#wrap{
  color: #999;
  width: 50%;
}
.wrap{
  color:#ccc;
}
#wrap{
  color:#666;
}
```

**属性**

```LESS
 /* LESS */
@borderStyle: border-style;
@Soild:solid;
#wrap{
  @{borderStyle}: @Soild;//变量名 必须使用大括号包裹
}
/* 生成的 CSS */
#wrap{
  border-style:solid;
}

```

**url**

```LESS
/* LESS */
@images: "../img";//需要加引号
body {
  background: url("@{images}/dog.png");//变量名 必须使用大括号包裹
}
/* 生成的 CSS */
body {
  background: url("../img/dog.png");
}
```

## 2.3. 申明变量

定义一组属性值，直接 `@name()` 进行调用

```LESS
/* LESS */
@background: {background:red;};
#main{
    @background();
}
@Rules:{
    width: 200px;
    height: 200px;
    border: solid 1px red;
};
#con{
  @Rules();
}

/* 生成的 CSS */
#main{
  background:red;
}
#con{
  width: 200px;
  height: 200px;
  border: solid 1px red;
}
```

## 2.4. 变量运算

加减法时，以第一个数据的单位为基准。乘除法时，单位一定要统一。

```LESS
 /* Less */
@width:300px;
@color:#222;
#wrap{
  width:@width-20;
  height:@width-20*5;
  margin:(@width-20)*5;
  color:@color*2;
  background-color:@color + #111;
}
/* 生成的 CSS */
#wrap{
  width:280px;
  height:200px;
  margin:1400px;
  color:#444;
  background-color:#333;
}
```

## 2.5. 变量定义变量

解析的顺序是从后向前逐层解析。

```LESS
/* Less */
@fnord:  "I am fnord.";
@var:    "fnord";
#wrap::after{
  content: @@var; //将@var替换为其值 content:@fnord;
}
/* 生成的 CSS */
#wrap::after{
  content: "I am fnord.";
}
```

## 2.6. 变量范围

变量范围指定可用变量的位置。 变量将从本地作用域搜索，如果它们不可用，则编译器将从父作用域搜索

```LESS
@var: @a;
@a: 15px;

.myclass {
  font-size: @var;
  @a:20px;
  color: green;
}
/* 生成的 CSS */
.myclass {
  font-size: 20px;
  color: green;
}
```

# 3. 嵌套

直接在属性内定义子元素样式，`&` 符号代表上一级选择器

```LESS
/* Less */
#header{
  &:after{ //注意：不能省略&，如果省略会给每一个子元素添加一个after。
    content:"Less is more!";
  }
  .title{
    font-weight:bold;
  }
  &_content{//理解方式：直接把 & 替换成 #header
    margin:20px;
  }
}
/* 生成的 CSS */
#header::after{
  content:"Less is more!";
}
#header .title{ //嵌套了
  font-weight:bold;
}
#header_content{//没有嵌套！
    margin:20px;
}
```

# 4. 媒体查询

可以直接在选择器内声明 特定媒体 的样式

```LESS
/* Less */
#main{
    //something...
    @media screen{
        @media (max-width:768px){
          width:100px;
        }
    }
    @media tv {
      width:2000px;
    }
}
/* 生成的 CSS */
@media screen and (max-width:768px){
  #main{
      width:100px;
  }
}
@media tv{
  #main{
    width:2000px;
    }
}
```

# 5. 混合

可以使用`.`或`#`作为方法前缀，定义混合方法。

## 5.1. 传递参数

`Less` 可以使用默认参数，如果没有传参数，那么将使用默认参数。 `@arguments` 犹如 JS 中的 `arguments` 指代的是全部参数。 传的参数中 必须带着单位。

```less
/* Less */
.border(@a:10px,@b:50px,@c:30px,@color:#000) {
  border: solid 1px @color;
  box-shadow: @arguments; //指代的是 全部参数
}
#main {
  .border(0px,5px,30px,red); //必须带着单位
}
#wrap {
  .border(0px);
}
#content {
  .border; //等价于 .border()
}

/* 生成的 CSS */
#main {
  border: solid 1px red;
  box-shadow: 0px, 5px, 30px, red;
}
#wrap {
  border: solid 1px #000;
  box-shadow: 0px 50px 30px #000;
}
#content {
  border: solid 1px #000;
  box-shadow: 10px 50px 30px #000;
}
```

## 5.2. 不定参

不确定参数的个数

```LESS
 /* Less */
.boxShadow(...){
    box-shadow: @arguments;
}
.textShadow(@a,...){
    text-shadow: @arguments;
}
#main{
    .boxShadow(1px,4px,30px,red);
    .textShadow(1px,4px,30px,red);
}
/* 生成后的 CSS */
#main{
  box-shadow: 1px 4px 30px red;
  text-shadow: 1px 4px 30px red;
}
```

## 5.3. 匹配模式

同一个方法名的多个方法，由于传入的参数不同而实现不同的功能。

```LESS
 /* Less */
.triangle(top,@width:20px,@color:#000){
    border-color:transparent  transparent @color transparent ;
}
.triangle(right,@width:20px,@color:#000){
    border-color:transparent @color transparent  transparent ;
}

.triangle(bottom,@width:20px,@color:#000){
    border-color:@color transparent  transparent  transparent ;
}
.triangle(left,@width:20px,@color:#000){
    border-color:transparent  transparent  transparent @color;
}
.triangle(@_,@width:20px,@color:#000){
    border-style: solid;
    border-width: @width;
}
#main{
    .triangle(left, 50px, #999)
}
/* 生成的 CSS */
#main{
  border-color:transparent  transparent  transparent #999;
  border-style: solid;
  border-width: 50px;
}
```

## 5.4. 命名空间

可以在混合中嵌套混合，使用时需要链式声明。如果使用 `>` 选择器，父元素不能加括号

```LESS
 /* Less */
#card(){
    background: #723232;
    .d(@w:300px){
        width: @w;
        #a(@h:300px){
            height: @h;//可以使用上一层传进来的方法
            width: @w;
        }
    }
}
#wrap{
    #card > .d > #a(100px); // 父元素不能加 括号
}
#main{
    #card .d();
}
#con{
    //不得单独使用命名空间的方法
    //.d() 如果前面没有引入命名空间 #card ，将会报错
    #card; // 等价于 #card();
    .d(20px); //必须先引入 #card
}
/* 生成的 CSS */
#wrap{
  height:100px;
  width:300px;
}
#main{
  width:300px;
}
#con{
  width:20px;
}
```

## 5.5. 条件语句

混合可以使用 `when`，`and`，`not` 和 `,(||)` 来控制使用

```LESS
 /* Less */
#card{
    // and 运算符 ，相当于 与运算 &&，必须条件全部符合才会执行
    .border(@width,@color,@style) when (@width>100px) and(@color=#999){
        border:@style @color @width;
    }
    // not 运算符，相当于 非运算 !，条件为 不符合才会执行
    .background(@color) when not (@color>=#222){
        background:@color;
    }
    // , 逗号分隔符：相当于 或运算 ||，只要有一个符合条件就会执行
    .font(@size:20px) when (@size>50px) , (@size<100px){
        font-size: @size;
    }
}
#main{
    #card>.border(200px,#999,solid);
    #card .background(#111);
    #card > .font(40px);
}
/* 生成后的 CSS */
#main{
  border:solid #999 200px;
  background:#111;
  font-size:40px;
}
```

## 5.6. 条件运算符

比较运算有：`>` `>=` `=` `=<` `<`

`=` 代表是等于

除去关键字 `true` 以外的值其他都会被默认为 `fales`

## 5.7. 循环

less 没有 for 循环，可以使用递归实现循环

```LESS
/* Less */
.generate-columns(4);
.generate-columns(@n, @i: 1) when (@i =< @n) {
  .column-@{i} {
    width: (@i * 100% / @n);
  }
  .generate-columns(@n, (@i + 1));
}
/* 生成后的 CSS */
.column-1 {
  width: 25%;
}
.column-2 {
  width: 50%;
}
.column-3 {
  width: 75%;
}
.column-4 {
  width: 100%;
}
```

# 6. 属性拼接

## 6.1. 空格拼接

`+_`代表的是空格。

```LESS
/* Less */
.Animation() {
  transform+_: scale(2);
}
.main {
  .Animation();
  transform+_: rotate(15deg);
}

/* 生成的 CSS */
.main {
  transform: scale(2) rotate(15deg);
}
```

## 6.2. 逗号拼接

`+`代表的是逗号

```less
/* Less */
.boxShadow() {
  box-shadow+: inset 0 0 10px #555;
}
.main {
  .boxShadow();
  box-shadow+: 0 0 20px black;
}
/* 生成后的 CSS */
.main {
  box-shadow: inset 0 0 10px #555, 0 0 20px black;
}
```

# 7. 继承

`extend` 是 `less` 的一个伪类。它可以继承所匹配声明中的全部样式。

**:extend(#select all)** 可以继承所有声明

```LESS
 /* Less */
.animation{
    transition: all .3s ease-out;
    .hide{
      transform:scale(0);
    }
}
#main{
    &:extend(.animation);
}
#con{
    &:extend(.animation .hide);
}

/* 生成后的 CSS */
.animation,#main{
  transition: all .3s ease-out;
}
.animation .hide , #con{
    transform:scale(0);
}
```

# 8. 导入

在 `less` 文件中可以引入其他的 `less` 文件。使用关键字 `import`。

```LESS
@import 'index.less';
@import 'index';
```

# 9. 函数

1. unit：更改尺寸单位，`unit(30, px)=>30px`
2. ceil: 它将数字向上舍入为下一个最大整数。
3. floor:它将数字向下取整为下一个最小整数。
