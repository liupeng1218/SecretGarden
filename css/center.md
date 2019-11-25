<!-- TOC -->

- [水平居中](#%e6%b0%b4%e5%b9%b3%e5%b1%85%e4%b8%ad)
  - [text-algin](#text-algin)
  - [margin](#margin)
  - [positon margin](#positon-margin)
  - [positon transform](#positon-transform)
  - [flex](#flex)
  - [小结](#%e5%b0%8f%e7%bb%93)
- [垂直居中](#%e5%9e%82%e7%9b%b4%e5%b1%85%e4%b8%ad)
  - [line-height](#line-height)
  - [vertical-align](#vertical-align)
  - [table-cell](#table-cell)
  - [positon margin](#positon-margin-1)
  - [positon transform](#positon-transform-1)
  - [flex](#flex-1)
  - [小结](#%e5%b0%8f%e7%bb%93-1)
- [水平垂直居中](#%e6%b0%b4%e5%b9%b3%e5%9e%82%e7%9b%b4%e5%b1%85%e4%b8%ad)
  - [text-algin&line-height](#text-alginline-height)
  - [table-cell](#table-cell-1)
  - [positon margin](#positon-margin-2)
  - [positon transform](#positon-transform-2)
  - [](#)
  - [flex](#flex-2)
  - [小结](#%e5%b0%8f%e7%bb%93-2)

<!-- /TOC -->

> 参考文档 [MDN](https://developer.mozilla.org) [兼容性](https://caniuse.com/)

[线上demo](https://liupeng1218.github.io/SecretGardenDemo/css/center)

# 水平居中

## text-algin

[text-algin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align) 定义行内内容如何相对它的块父元素对齐

```CSS
.parnent{
  text-algin:center;
}
```

优点

- 定义简单，易于理解
- 兼容性好

缺点

- 只对行内内容有效
- 子元素宽度大于父元素宽度则失效

## margin

[margin](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin) 设置为`auto`时，浏览器会选择一个合适的`margin`，即会均分剩余空间。上下的`margin`设置`auto`，其计算值为 0

```CSS
.child{
  width: 100px;
  margin: 0 auto;
}
```

优点

- 定义简单，易于理解
- 兼容性好

缺点

- 元素必须固定宽度
- 元素宽度要小于父元素

## positon margin

利用`positon`子绝父相和`margin`组合达到水平居中的目的

```CSS
.parent{
  position: relative;
}
.child{
  position: absolute;
  width: 100px;
  left: 50%;
  margin-left: -50px;
}
```

优点

- 定义简单，易于理解
- 兼容性好

缺点

- 元素必须固定宽度
- 元素会脱离文档流

## positon transform

利用`positon`子绝父相和`transform`组合达到水平居中的目的

```CSS
.parent{
  position: relative;
}
.child{
  position: absolute;
  width: 100px;
  left: 50%;
  transform: translateX(-50%);
}
```

优点

- 定义简单，易于理解
- 利用`translate`可以对非固定宽度元素配置

缺点

- 元素会脱离文档流
- `translate`有一定的兼容性问题

## flex

`flex`容器的子元素会成为伸缩项目，设置其主轴的排列方式，可以使其子元素达到居中效果

```CSS
.parent{
  position: flex;
  justify-content: center;
}
```

优点

- 定义简单，易于理解
- 子元素尺寸，个数都不受限制

缺点

- `flex`有一定的兼容性问题

## 小结

1. 简单场景优先考虑`text-algin`和`margin`，结构简单，兼容性好
2. `position`和`margin`/`transform`可以对外层布局及子元素不定宽情况进行处理
3. `flex`简单灵活，且不仅适用于居中，功能非常强大。在移动端或对兼容不强要求情况下推荐使用

# 垂直居中

## line-height

利用`line-height`和`height`组合实现居中效果

```CSS
.parent{
  height:200px;
  line-height:200px;
}
```

优点

- 定义简单，易于理解
- 兼容性好

缺点

- 只适用于单行行内元素
- 需要定高

## vertical-align

利用[`vertical-align`和`line-height`](http://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/)的关系实现图片的居中

```CSS
.parent {
  height:200px;
  line-height: 200px;
  font-size: 0;
}
.parent img {
  vertical-align: middle;
}
```

优点

- 定义简单，易于理解
- 兼容性好

缺点

- 需要`font-size`配合才能实现绝的的垂直居中
- 只适用于单行行内元素

## table-cell

指定元素作为表格单元格来处理，和`<td>`标签有相同的特性

```CSS
.parent {
  display: table-cell;
  vertical-align: middle;
}
```

优点

- 定义简单，易于理解
- 适用于高度位置情况

缺点

- 设置`tabl-cell`的元素，会有宽度和高度的值设置百分比无效，不感知 margin 等隐形特性

## positon margin

同水平居中，利用`positon`子绝父相和`margin`组合达到水平居中的目的

```CSS
.parent{
  position: relative;
}
.child{
  position: absolute;
  width: 100px;
  height: 100px;
  top: 50%;
  margin-top: -50px;
}
/* 当positon 的top,bottom属性设置为0时，margin的auto会自动分配空间 */
.child{
  position: absolute;
  width: 100px;
  height: 100px;
  top: 0;
  bottom: 0;
  margin: auto 0;
}
```

优点

- 定义简单，易于理解
- 兼容性好

缺点

- 元素必须固定尺寸
- 元素会脱离文档流

## positon transform

同水平居中，利用`positon`子绝父相和`transform`组合达到水平居中的目的

```CSS
.parent{
  position: relative;
}
.child{
  position: absolute;
  width: 100px;
  height: 100px;
  top: 50%;
  transform: translateY(-50%);
}
```

优点

- 定义简单，易于理解
- 利用`translate`可以对非固定宽度元素配置

缺点

- 元素会脱离文档流
- `translate`有一定的兼容性问题

## flex

`flex`功能非常灵活，设置其排列方式，对齐方式可以实现垂直居中

```CSS
.parent{
  position: flex;
  align-items: center;
}
```

优点

- 定义简单，易于理解
- 子元素尺寸不受限制，单行多行都可以实现

缺点

- `flex`有一定的兼容性问题

## 小结

1. 单行行内内容优先考虑`line-height`结构简单，兼容性好
2. `position`和`margin`/`transform`可以对外层布局及子元素不定高情况进行处理
3. `flex`简单灵活，且不仅适用于居中，功能非常强大。在移动端或对兼容不强要求情况下推荐使用

# 水平垂直居中

## text-algin&line-height

`text-algin`配合`line-height`实现单行行内元素的水平垂直居中，`font-size:0`消除图片近似居中的 bug

```CSS
.parent {
  height: 200px;
  line-height: 200px;
  font-size: 0;
  text-align: center;
}
.parent img{
  vertical-align: middle;
}
.child{
  font-size: 16px;
}
```

优点

- 定义简单，易于理解
- 兼容性好

缺点

- 只会单行行内元素有效

## table-cell

利用`table-cell`单元格特性实现居中

```CSS
.parent {
  height: 200px;
  line-height: 200px;
  display: table-cell;
  text-align: center; /*行内级元素就添加margin*/
  vertical-align: middle;
}
.child{
  width:100px;
  height:100px;
  margin: 0 auto; /*块级元素就添加margin*/
}
```

优点

- 定义简单，易于理解
- 兼容性好
- 适用于尺寸未定元素

缺点

- 设置`tabl-cell`的元素，会有宽度和高度的值设置百分比无效，不感知 margin 等隐形特性

## positon margin

利用`positon`子绝父相和`margin`组合达到水平垂直居中的目的

```CSS
.parent{
  position: relative;
}
.child{
  position: absolute;
  width: 100px;
  left: 50%;
  top: 50%;
  margin-left: -50px;
  margin-top: -50px;
}
/* 当top、bottom为0时,margin-top&bottom设置auto的话会无限延伸占满空间并且平分；当left、right为0时,margin-left&right设置auto的话会无限延伸占满空间并且平分 */
.child{
  position: absolute;
  width: 100px;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  margin:auto;
}
```

优点

- 定义简单，易于理解
- 兼容性好

缺点

- 元素必须固定宽度
- 元素会脱离文档流

## positon transform

利用`positon`子绝父相和`transform`组合达到水平垂直居中的目的

```CSS
.parent{
  position: relative;
}
.child{
  position: absolute;
  width: 100px;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
```

优点

- 定义简单，易于理解
- 利用`translate`可以对非固定宽度元素配置

缺点

- 元素会脱离文档流
- `translate`有一定的兼容性问题

## 

## flex

`flex`容器的子元素会成为伸缩项目，设置其主轴和侧轴的排列方式可以使其子元素达到水平垂直居中效果

```CSS
.parent{
  position: flex;
  justify-content: center;
  align-items: center;
}
```

优点

- 定义简单，易于理解
- 子元素尺寸，个数都不受限制

缺点

- `flex`有一定的兼容性问题


## 小结

1. 简单行内场景优先考虑`text-algin`和`line-height`的结合，结构简单，兼容性好
2. `position`和`margin`/`transform`是对其水平居中和垂直居中的组合，可以应用于绝大部分场景
3. `flex`功能强大，在移动端或对兼容不强要求情况下推荐优先使用