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
  - [两列定宽，一列自适应](#%e4%b8%a4%e5%88%97%e5%ae%9a%e5%ae%bd%e4%b8%80%e5%88%97%e8%87%aa%e9%80%82%e5%ba%94)
    - [float margin](#float-margin-1)
    - [float overflow](#float-overflow-2)
    - [position](#position-1)
    - [table](#table-1)
    - [flex](#flex-2)
    - [grid](#grid-2)
  - [两列顶宽，中间自适应](#%e4%b8%a4%e5%88%97%e9%a1%b6%e5%ae%bd%e4%b8%ad%e9%97%b4%e8%87%aa%e9%80%82%e5%ba%94)
    - [双飞翼布局](#%e5%8f%8c%e9%a3%9e%e7%bf%bc%e5%b8%83%e5%b1%80)
    - [圣杯布局](#%e5%9c%a3%e6%9d%af%e5%b8%83%e5%b1%80)
    - [table](#table-2)
    - [positon](#positon)
    - [flex](#flex-3)
    - [grid](#grid-3)
  - [小结](#%e5%b0%8f%e7%bb%93-1)
- [多列布局](#%e5%a4%9a%e5%88%97%e5%b8%83%e5%b1%80)
  - [等宽布局](#%e7%ad%89%e5%ae%bd%e5%b8%83%e5%b1%80)
    - [float](#float)
    - [table](#table-3)
    - [flex](#flex-4)
    - [grid](#grid-4)
  - [小结](#%e5%b0%8f%e7%bb%93-2)
- [栅格系统](#%e6%a0%85%e6%a0%bc%e7%b3%bb%e7%bb%9f)
- [九宫格布局](#%e4%b9%9d%e5%ae%ab%e6%a0%bc%e5%b8%83%e5%b1%80)
  - [table](#table-4)
  - [flex](#flex-5)
  - [grid](#grid-5)
  - [小结](#%e5%b0%8f%e7%bb%93-3)

<!-- /TOC -->

[两列布局线上 demo](https://liupeng1218.github.io/SecretGardenDemo/css/layout-two_columns.html)
[三列布局线上 demo](https://liupeng1218.github.io/SecretGardenDemo/css/layout-three_columns.html)
[多列布局线上 demo](https://liupeng1218.github.io/SecretGardenDemo/css/layout-multi_columns.html)
[九宫格布局线上 demo](https://liupeng1218.github.io/SecretGardenDemo/css/layout-sudoku.html)

> 参考 Sweet-KK 的[布局分析](https://github.com/Sweet-KK/css-layout)
> 阮一峰老师[Flex 布局](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)
> 阮一峰老师[CSS Grid 网格布局](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

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

- 结构简单
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

- 易于理解，布局灵活
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

利用`overflow`触发 BFC 不需要设置宽度达到根据视窗和不定列来自适应的效果

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
      .method-ten{
        display: grid;
        grid-template-columns: auto 1fr;
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

公共样式

```CSS
      .item {
        height: 300px;
        margin-bottom: 20px;
      }
      .left {
        height: 100%;
        background-color: #dff6f0;
      }
      .middle {
        height: 100%;
        background-color: #46b3e6;
      }
      .right {
        height: 100%;
        background-color: #4d80e4;
      }
```

## 两列定宽，一列自适应

### float margin

利用`float`浮动定宽列，自适应列做相应的偏移

```HTML
    <div class="item method-one">
      <div class="left">float</div>
      <div class="middle">float</div>
      <div class="right">margin</div>
    </div>
```

```CSS
      .method-one .left {
        float: left;
        width: 200px;
      }
      .method-one .middle {
        float: left;
        width: 200px;
      }
      .method-one .right{
        margin-left: 200px;
      }
```

优点

- 结构简单，容易理解
- 兼容性好

缺点

- 父元素宽度过小时，浮动元素会被挤脱换行
- 自适应元素偏移需要与定宽元素配合

### float overflow

利用`float`浮动定宽列，自适应列触发 BFC 达到自适应

```HTML
    <div class="item method-two">
      <div class="left">float</div>
      <div class="middle">float</div>
      <div class="right">margin</div>
    </div>
```

```CSS
      .method-two .left {
        float: left;
        width: 200px;
      }
      .method-two .middle {
        float: left;
        width: 200px;
      }
      .method-two .right{
        overflow: hidden;
      }
```

优点

- 结构简单，容易理解
- 兼容性好
- 不需要考虑自适应元素与定宽元素的偏移配合

缺点

- 父元素宽度过小时，浮动元素会被挤脱换行
- 自适应元素的`margin-left`效果需要大于定宽元素宽度才会生效

### position

利用`position`直接设置三列的位置与尺寸

```HTML
    <div class="item method-three">
      <div class="left">left</div>
      <div class="middle">middle</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-three {
        position: relative;
      }
      .method-three .left {
        position: absolute;
        top: 0;
        left: 0;
        width: 200px;
      }
      .method-three .middle {
        position: absolute;
        top: 0;
        left: 200px;
        width: 200px;
      }
      .method-three .right {
        position: absolute;
        top: 0;
        left: 400px;
        right: 0;
      }
```

优点

- 容易理解，布局灵活
- 兼容性好

缺点

- 需要计算与固定各列的位置与尺寸

### table

利用`table`特性以表格形式布局

```HTML
    <div class="item method-four">
      <div class="left">left</div>
      <div class="middle">middle</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-four {
        width: 100%;
        display: table;
      }
      .method-four .left {
        display: table-cell;
        width: 200px;
      }
      .method-four .middle {
        display: table-cell;
        width: 200px;
      }
      .method-four .right {
        display: table-cell;
      }
```

优点

- 结构简单，兼容性好
- 可以处理尺寸位置的场景

缺点

- `table`结构有很多如`margin`失效等隐性特性需要注意

### flex

利用`flex`布局，配置自适应列的扩张比例

```HTML
    <div class="item method-five">
      <div class="left">left</div>
      <div class="middle">middle</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-five {
        display: flex;
      }
      .method-five .left {
        width: 200px;
      }
      .method-five .middle {
        width: 200px;
      }
      .method-five .right {
        flex: 1;
      }
```

优点

- 结构简单，容易理解
- 可以灵活处理多重布局场景

缺点

- 有一定兼容性问题

### grid

利用`grid`布局方式，直接对不同列配置相应的布局结果

```HTML
    <div class="item method-six">
      <div class="left">left</div>
      <div class="middle">middle</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-six {
        display: grid;
        grid-template-columns: 200px 200px 1fr;
      }
```

优点

- 结构简单，布局灵活

缺点

- 有一定兼容性问题

## 两列顶宽，中间自适应

### 双飞翼布局

布局关键在于中间自适应列宽采用双层结构，宽度设置`100%`达到自适应效果
内层设置`margin`将两侧内容的宽度让出来
两侧定宽列使用`margin-left`偏移到两侧位置
**注意 html 结构顺序**

```HTML
    <div class="item method-seven">
      <div class="middle">
        <div class="middle__content">middle</div>
      </div>
      <div class="left">left</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-seven .left {
        float: left;
        width: 200px;
        margin-left: -100%;
      }
      .method-seven .middle {
        float: left;
        width: 100%;
      }
      .method-seven .middle__content {
        margin: 0 200px 0 200px;
      }
      .method-seven .right {
        float: left;
        width: 200px;
        margin-left: -200px;
      }
```

优点

- 经典布局，兼容性好

缺点

- html 结构较多，注意布局顺序

### 圣杯布局

父元素设置`padding`为两个定宽列宽度，空余出两侧位置
中间设置宽度`100%`达到自适应效果
两侧内容使用`margin-left`移动到同一列
使用`relative`偏移到指定位置

**注意 html 结构顺序**

```HTML
    <div class="item method-eight">
      <div class="middle">middle</div>
      <div class="left">left</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-eight {
        padding: 0 200px;
      }
      .method-eight .left {
        width: 200px;
        float: left;
        margin-left: -100%;
        position: relative;
        left: -200px;
      }
      .method-eight .middle {
        width: 100%;
        float: left;
      }
      .method-eight .right {
        width: 200px;
        float: left;
        margin-left: -200px;
        position: relative;
        left: 200px;
      }
```

优点

- 代码简单，兼容性好

缺点

- 样式结构较多，注意布局顺序

### table

利用`table`结构的弹性特性实现布局效果

```HTML
    <div class="item method-nine">
      <div class="left">left</div>
      <div class="middle">middle</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-nine{
        width: 100%;
        display: table;
      }
      .method-nine .left{
        display: table-cell;
        width: 200px;
      }
      .method-nine .middle{
        display: table-cell;
      }
      .method-nine .right{
        display: table-cell;
        width: 200px;
      }
```

优点

- 代码简单，兼容性好

缺点

- `table`结构有很多如`margin`失效等隐性特性需要注意

### positon

利用`positon`对三列结构直接定位和布局

```HTML
    <div class="item method-ten">
      <div class="left">left</div>
      <div class="middle">middle</div>
      <div class="right">right</div>
    </div>
```

```CSS
      .method-ten {
        position: relative;
      }
      .method-ten .left {
        position: absolute;
        width: 200px;
        top: 0;
        left: 0;
      }
      .method-ten .middle {
        position: absolute;
        top: 0;
        right: 200px;
        left: 200px;
      }
      .method-ten .right {
        position: absolute;
        width: 200px;
        top: 0;
        right: 0;
      }
```

优点

- 易于理解
- 兼容性好
- 布局灵活

缺点

- 分别配置所有列位置，尺寸等

### flex

利用`flex`设置定宽列宽度和自适应列的扩张比例

```HTML
    <div class="item method-eleven">
      <div class="left">eleven-left</div>
      <div class="middle">eleven-middle</div>
      <div class="right">eleven-right</div>
    </div>
```

```CSS
      .method-eleven {
        display: flex;
      }
      .method-eleven .left {
        width: 200px;
      }
      .method-eleven .middle {
        flex: 1;
      }
      .method-eleven .right {
        width: 200px;
      }
```

优点

- 易于理解
- 布局灵活

缺点

- 有一定兼容性问题

### grid

利用`grid`新型布局方式直接对三列进行配置

```HTML
    <div class="item method-twelve">
      <div class="left">twelve-left</div>
      <div class="middle">twelve-middle</div>
      <div class="right">twelve-right</div>
    </div>
```

```CSS
      .method-twelve{
        display: grid;
        grid-template-columns: 200px 1fr 200px;
      }
```

优点

- 代码简单
- 布局灵活，功能强大

缺点

- 注意兼容性问题

## 小结

1. 两列定宽，一列自适应推荐使用`float`配合`margin/overflow`布局，结构简单，易于理解
2. 两侧定宽，中间自适应推荐双飞翼或圣杯布局，这两种布局方式需要对盒模型和浮动有一定的了解。圣杯布局在浏览器过窄时会破坏布局，需要进行处理。
3. table 布局适用三列的常见情况，使用时需要了解`table`的隐性特性的干扰
4. `position`可以直接对布局进行灵活定位，使用时会脱离文档流，注意父元素的高度
5. `flex`和`grid`是新型的布局方式，功能强大，布局灵活，在兼容性允许的情况下推荐使用

# 多列布局

公共样式

```CSS
      .item {
        height: 300px;
        margin-bottom: 20px;
      }
      .item > div {
        height: 100%;
        box-sizing: border-box;
        background-clip: content-box;
      }
      .item > div:nth-child(even) {
        background-color: #dff6f0;
      }
      .item > div:nth-child(odd) {
        background-color: #4d80e4;
      }
```

## 等宽布局

### float

根据元素个数平均分配宽度，如果需要间隔，使用`padding`模拟，同时需要父元素`margin-left`抵消掉最左侧的偏移

```HTML
    <div class="item method-one">
      <div class="child"></div>
      <div class="child"></div>
      <div class="child"></div>
      <div class="child"></div>
      <div class="child"></div>
    </div>
```

```CSS
      .method-one {
        margin-left: -10px;
      }
      .method-one .child{
        float: left;
        width: 20%;
        padding-left: 10px;
      }
```

优点

- 代码简单
- 兼容性好

缺点

- 只能在确定列数情况下使用
- 需要清除浮动带来的塌陷等问题

### table

利用`table`元素均分宽度的特性实现等宽布局

```HTML
    <div class="item method-two">
      <div class="child">child</div>
      <div class="child">child</div>
      <div class="child">child</div>
      <div class="child">child</div>
      <div class="child">child</div>
    </div>
```

```CSS
      .method-two {
        width: 100%;
        display: table;
      }
      .method-two .child {
        display: table-cell;
      }
```

优点

- 代码简单
- 兼容性好
- 可以处理高度未知情况

缺点

- 需要处理 table 样式特性带来的问题

### flex

利用`felx`统一元素的扩张比例达到等宽的效果

```HTML
    <div class="item method-three">
      <div class="child">three</div>
      <div class="child">three</div>
      <div class="child">three</div>
      <div class="child">three</div>
      <div class="child">three</div>
    </div>
```

```CSS
      .method-three {
        display: flex;
      }
      .method-three .child {
        flex: 1;
      }
```

优点

- 代码简单
- 布局灵活
- 可以处理任意列，不需要设置宽度

缺点

- 有一定的兼容性问题

### grid

利用`grid`设置列的布局比例

```HTML
    <div class="item method-four">
      <div class="child">four</div>
      <div class="child">four</div>
      <div class="child">four</div>
      <div class="child">four</div>
      <div class="child">four</div>
    </div>
```

```CSS
      .method-four {
        display: grid;
        grid-template-columns: repeat(5,1fr);
      }
```

优点

- 代码简单
- 布局灵活，功能强大

缺点

- 需要处理兼容性问题

## 小结

1. 多列布局最常用是通过`float`进行均分，注意模拟间隔时，计算好间隔或者抵消偏移
2. `table`布局可以任意划分列，使用时需要注意其隐性特性对布局的影响
3. `flex`使用方便，功能强大，兼容性允许的情况下推荐使用

# 栅格系统

栅格系统是一种非常灵活的布局方式，许多框架如`bootstrap`等都对其进行了实现
简易版

```LESS
/*生成栅格系统*/
@media screen and (max-width: 768px){
  .generate-columns(@n, @i: 1) when (@i <= @n) {
    .column-xs-@{i} {
      width: (@i * 100% / @n);
    }
    .generate-columns(@n, (@i+1));
  }
  .generate-columns(12);     /*此处设置生成列数*/
}
@media screen and (min-width: 768px){
  .generate-columns(@n, @i: 1) when (@i <= @n) {
    .column-sm-@{i} {
      width: (@i * 100% / @n);
    }
    .generate-columns(@n, (@i+1));
  }
  .generate-columns(12);    /*此处设置生成列数*/
}
div[class^="column-xs-"]{
	float: left;
}
div[class^="column-sm-"]{
	float: left;
}
```

```CSS
@media screen and (max-width: 768px) {
  .column-xs-1 {  width: 8.33333333%;  }
  .column-xs-2 {  width: 16.66666667%;  }
  .column-xs-3 {  width: 25%;  }
  .column-xs-4 {  width: 33.33333333%;  }
  .column-xs-5 {  width: 41.66666667%;  }
  .column-xs-6 {  width: 50%;  }
  .column-xs-7 {  width: 58.33333333%;  }
  .column-xs-8 {  width: 66.66666667%;  }
  .column-xs-9 {  width: 75%;  }
  .column-xs-10 {  width: 83.33333333%;  }
  .column-xs-11 {  width: 91.66666667%;  }
  .column-xs-12 {  width: 100%;  }
}
@media screen and (min-width: 768px) {
  .column-sm-1 {  width: 8.33333333%;  }
  .column-sm-2 {  width: 16.66666667%;  }
  .column-sm-3 {  width: 25%;  }
  .column-sm-4 {  width: 33.33333333%;  }
  .column-sm-5 {  width: 41.66666667%;  }
  .column-sm-6 {  width: 50%;  }
  .column-sm-7 {  width: 58.33333333%;  }
  .column-sm-8 {  width: 66.66666667%;  }
  .column-sm-9 {  width: 75%;  }
  .column-sm-10 {  width: 83.33333333%;  }
  .column-sm-11 {  width: 91.66666667%;  }
  .column-sm-12 {  width: 100%;  }
}
div[class^="column-xs-"]{
	float: left;
}
div[class^="column-sm-"]{
	float: left;
}
```

# 九宫格布局

## table

使用`table`布局均分均分行和列

```HTML
    <div class="item method-one">
      <div class="row">
        <div class="cell">1</div>
        <div class="cell">2</div>
        <div class="cell">2</div>
      </div>
      <div class="row">
        <div class="cell">4</div>
        <div class="cell">5</div>
        <div class="cell">6</div>
      </div>
      <div class="row">
        <div class="cell">7</div>
        <div class="cell">8</div>
        <div class="cell">9</div>
      </div>
    </div>
```

```CSS
      .method-one {
        width: 300px;
        height: 300px;
        display: table;
      }
      .method-one .row {
        display: table-row;
      }
      .method-one .cell {
        display: table-cell;
        border: 1px solid #eee;
      }
```

优点

- 结构简单，易于理解
- 兼容性好

缺点

- 需要考虑`table`的隐性特性

## flex

使用`flex`布局，对伸缩元素进行换行均分

```HTML
    <div class="item method-two">
      <div class="child">1</div>
      <div class="child">2</div>
      <div class="child">3</div>
      <div class="child">4</div>
      <div class="child">5</div>
      <div class="child">6</div>
      <div class="child">7</div>
      <div class="child">8</div>
      <div class="child">9</div>
    </div>
```

```CSS
      .method-two {
        width: 300px;
        height: 300px;
        display: flex;
        flex-wrap: wrap;
      }
      .method-two .child {
        width: 33.3333333%;
        border: 1px solid #eee;
        box-sizing: border-box;
      }
```

优点

- 结构简单，易于理解

缺点

- 有一定兼容性问题

## grid

使用`grid`布局，设置行和列的分隔数量和比例

```HTML
    <div class="item method-three">
      <div class="child">1</div>
      <div class="child">2</div>
      <div class="child">3</div>
      <div class="child">4</div>
      <div class="child">5</div>
      <div class="child">6</div>
      <div class="child">7</div>
      <div class="child">8</div>
      <div class="child">9</div>
    </div>
```

```CSS
      .method-three{
        width: 300px;
        height: 300px;
        display: grid;
        grid-template-columns: repeat(3,1fr);
        grid-template-rows: repeat(3,1fr);
      }
```

优点

- 结构简单，布局灵活

缺点

- 有一定兼容性问题

## 小结

1. 简单场景可以使用`table`实现，结构简单，兼容性好，需要注意其隐性特性
2. 兼容性允许的情况下，推荐`flex`和`grid`可以适应更多复杂布局，更为灵活
