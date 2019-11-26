<!-- TOC -->

- [两列布局](#%e4%b8%a4%e5%88%97%e5%b8%83%e5%b1%80)
  - [一列定宽，一列自适应](#%e4%b8%80%e5%88%97%e5%ae%9a%e5%ae%bd%e4%b8%80%e5%88%97%e8%87%aa%e9%80%82%e5%ba%94)
    - [float margin](#float-margin)
    - [float overflow](#float-overflow)
    - [table](#table)
    - [position](#position)
    - [flex](#flex)
    - [grid](#grid)
  - [一列不定，一列自适应](#%e4%b8%80%e5%88%97%e4%b8%8d%e5%ae%9a%e4%b8%80%e5%88%97%e8%87%aa%e9%80%82%e5%ba%94)
    - [float overflow](#float-overflow-1)
    - [flex](#flex-1)
    - [grid](#grid-1)
  - [小结](#%e5%b0%8f%e7%bb%93)
- [三列布局](#%e4%b8%89%e5%88%97%e5%b8%83%e5%b1%80)
  - [两列顶宽，一列自适应](#%e4%b8%a4%e5%88%97%e9%a1%b6%e5%ae%bd%e4%b8%80%e5%88%97%e8%87%aa%e9%80%82%e5%ba%94)
  - [两列顶宽，中间自适应](#%e4%b8%a4%e5%88%97%e9%a1%b6%e5%ae%bd%e4%b8%ad%e9%97%b4%e8%87%aa%e9%80%82%e5%ba%94)
- [多列布局](#%e5%a4%9a%e5%88%97%e5%b8%83%e5%b1%80)
  - [等宽布局](#%e7%ad%89%e5%ae%bd%e5%b8%83%e5%b1%80)
  - [九宫格布局](#%e4%b9%9d%e5%ae%ab%e6%a0%bc%e5%b8%83%e5%b1%80)
  - [栅格系统](#%e6%a0%85%e6%a0%bc%e7%b3%bb%e7%bb%9f)
- [全屏布局](#%e5%85%a8%e5%b1%8f%e5%b8%83%e5%b1%80)

<!-- /TOC -->

[线上 demo](https://liupeng1218.github.io/SecretGardenDemo/css/layout.html)

> 参考 Sweet-KK 的[布局分析](https://github.com/Sweet-KK/css-layout)

# 两列布局

两列布局是常见的基础布局之一，常见为一侧定宽，另一侧自适应。还有一种为一列不定宽，一列自适应。

公共样式

```CSS
.item {
  height: 500px;
  margin-bottom: 20px;
}
.left {
  height: 100%;
  background-color: #dff6f0;
}
.right {
  height: 100%;
  background-color: #46b3e6;
}
```

## 一列定宽，一列自适应

### float margin

左侧浮动脱离文档流，右侧设置和左侧相等的`margin-left`达到两列布局的效果

```HTML
    <div class="item method-one">
      <div class="left"></div>
      <div class="right"></div>
    </div>
```

```CSS
      .method-one .left {
        float: left;
        width: 200px;
      }
      .method-one .right {
        margin-left: 200px;
      }
```

当布局为右列定宽，左侧自适应时，需要做相应的调整。
将左侧自适应列设置宽度为`100%`达到自适应的目的，同时将其`margin-left`设置为负值的定宽宽度让出位置让右侧定宽列上移达到两列布局。外层容器需要设置`padding-left`或`margin-left`抵消掉自适应列的位移。

```HTML
    <div class="item method-seven">
      <div class="left">left</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-seven{
        padding-left: 200px;
      }
      .method-seven .left{
        float: left;
        width: 100%;
        margin-left: -200px;
      }
      .method-seven .right{
        float: right;
        width: 200px;
      }
```

优点

- 结构简单，易于理解
- 兼容性好

缺点

- 自适应栏`margin-left`需要与定宽列相同
- 自适应列设置`margin-left/right`时，需要大于定宽尺寸才有视觉效果

### float overflow

左侧浮动脱离文档流，右侧设置`overflow`触发 BFC 达到自适应

```HTML
    <div class="item method-two">
      <div class="left"></div>
      <div class="right"></div>
    </div>
```

```CSS
      .method-two .left {
        float: left;
        width: 200px;
      }
      .method-two .right {
        overflow: hidden;
      }
```

优点

- 结构简单，易于理解
- 兼容性好
- 不需要考虑定宽列宽度

### table

利用`table`特性实现

```HTML
    <div class="item method-three">
      <div class="left">table</div>
      <div class="right">table</div>
    </div>
```

```CSS
      .method-three {
        width: 100%;
        display: table;
      }
      .method-three .left {
        width: 200px;
        display: table-cell;
      }
      .method-three .right {
        display: table-cell;
      }
```

优点

- 结构简单，易于理解
- 兼容性好

缺点

- `table`结构有很多如`margin`失效等隐性特性需要注意

### position

利用`position`固定两列位置

```HTML
    <div class="item method-four">
      <div class="left">position</div>
      <div class="right">position</div>
    </div>
```

```CSS
      .method-four {
        position: relative;
      }
      .method-four .left {
        position: absolute;
        top: 0;
        left: 0;
        width: 200px;
      }
      .method-four .right {
        position: absolute;
        top: 0;
        right: 0;
        left: 200px;
      }
```

优点

- 结构简单，易于理解
- 兼容性好

缺点

- 脱离文档流
- 定宽列宽度需要与自适应列偏移对应

### flex

利用`flex`容器成员沿主轴排列的特性，设置排列对齐方式实现两列布局

```HTML
    <div class="item method-five">
      <div class="left">flex</div>
      <div class="right">flex</div>
    </div>
```

```CSS
      .method-five {
        display: flex;
      }
      .method-five .left {
        width: 200px;
      }
      .method-five .right {
        flex: 1;
      }
```

优点

- 结构简单，易于理解

缺点

- 有一定兼容性问题

### grid

利用`grid`布局可以简单实现两列布局

> 推荐阮一峰老师的[CSS Grid 网格布局教程](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

```HTML
    <div class="item method-six">
      <div class="left">grid</div>
      <div class="right">grid</div>
    </div>
```

```CSS
      .method-six {
        display: grid;
        grid-template-columns: 200px 1fr;
      }
```

优点

- 结构简单
- `grid`是最新型的布局方式，可实现多种页面布局需求

缺点

- 兼容性问题比较严重

## 一列不定，一列自适应

### float overflow

利用`overflow`触发BFC不需要设置宽度达到根据视窗和不定列来自适应的效果

```HTML
    <div class="item method-eight">
      <div class="left"></div>
      <div class="right"></div>
    </div>
```

```CSS
      .method-eight .left {
        float: left;
      }
      .method-eight .right {
        overflow: hidden;
      }
```

优点

- 结构简单，易于理解
- 兼容性好
- 不需要考虑定宽列宽度

### flex

利用`flex`配置自适应列占满剩余空间

```HTML
    <div class="item method-nine">
      <div class="left"></div>
      <div class="right"></div>
    </div>
```

```CSS
      .method-nine{
        display: flex;
      }
      .method-nine .right{
        flex: 1;
      }
```

优点

- 结构简单，功能灵活

缺点
- 需要考虑兼容性

### grid

利用`grid`布局可以灵活配置每列的布局

```HTML
    <div class="item method-ten">
      <div class="left"></div>
      <div class="right"></div>
    </div>
```

```CSS
      .method-nine{
        display: flex;
      }
      .method-nine .right{
        flex: 1;
      }
```

优点

- 结构简单，功能灵活

缺点
- 兼容性较差

## 小结
1. 常见的一侧定宽，一侧自适应布局，可以使用`float`配合`margin/overflow`实现
2. 也可以使用绝对定位直接对两列进行定位，元素会脱离文档流
3. `table`布局有很多隐性属性，使用时需要对`table`的特性有一定了解
4. `flex`简单灵活，功能强大，在兼容性支持的情况下推荐优先使用


# 三列布局

## 两列顶宽，一列自适应

## 两列顶宽，中间自适应

# 多列布局

## 等宽布局

## 九宫格布局

## 栅格系统

# 全屏布局
