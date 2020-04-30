<!-- TOC -->

- [1. flex](#1-flex)
- [2. 容器](#2-%e5%ae%b9%e5%99%a8)
- [3. 子项](#3-%e5%ad%90%e9%a1%b9)

<!-- /TOC -->

> 参考 [写给自己看的 display: flex 布局教程](https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/),[30 分钟学会 Flex 布局](https://zhuanlan.zhihu.com/p/25303493),[理解 Flexbox：你需要知道的一切](https://www.w3cplus.com/css3/understanding-flexbox-everything-you-need-to-know.html)

# 1. flex

![flex](../assets/images/css-flex.jpg)

`flex` 是 `Flexible Box` 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

在 `flex` 容器中默认存在两条轴，默认水平主轴(main axis) 和垂直的交叉轴(cross axis)可以通过修改使垂直方向变为主轴，水平方向变为交叉轴。

`flex` 的属性分别有作用与容器和子项，都是用来控制`flex-item`的呈现

| flex 容器       | flex 子项   |
| --------------- | ----------- |
| flex-direction  | order       |
| flex-wrap       | flex-grow   |
| flex-flow       | flex-shrink |
| justify-content | flex-basis  |
| align-items     | flex        |
| align-content   | align-self  |

**当时设置 flex 布局之后，子元素的 float、clear、vertical-align 的属性将会失效**

# 2. 容器

任何一个容器都可以被指定为 `flex` 布局，这样容器内部的元素就可以使用 `flex` 来进行布局。

1. `flex-direction`: 用来控制子项整体布局方向，是从左往右还是从右往左，是从上往下还是从下往上
2. `flex-wrap`: 用来控制子项整体单行显示还是换行显示
3. `flex-flow`: 是`flex-direction`和`flex-wrap`的缩写，表示`flex`布局的`flow`流动特性
4. `justify-content`: 决定了水平方向子项的对齐和分布方式
5. `align-items`: flex 子项们相对于 flex 容器在垂直方向上的对齐方式
6. `align-content`: 定义了多根轴线的对齐方式，**如果项目只有一根轴线，那么该属性将不起作用**

# 3. 子项

容器的所有子元素都是容器成员，是 flex 子项

1. `order`: 定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0
2. `flex-basis`: 定义了在分配多余空间之前，项目占据的主轴空间，浏览器根据这个属性，计算主轴是否有多余空间。当主轴为水平方向的时候，当设置了 `flex-basis`，项目的宽度设置值会**失效**，`flex-basis` 需要跟 `flex-grow` 和 `flex-shrink` 配合使用才能发挥效果。
3. `flex-grow`: 定义项目的放大比例，默认值为 0
4. `flex-shrink`: 定义了项目的缩小比例，默认值为 1，即容器空间不足时，项目会缩小
5. `flex`: `flex-grow` , `flex-shrink` 和 `flex-basis` 的简写。该属性有许多快捷值
   1. auto：1 1 auto
   2. none: 0 0 auto
   3. 当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%
   4. 当 flex 取值为 0 时，对应的三个值分别为 0 1 0%
   5. 当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1，有如下等同情况（注意 0% 是一个百分比而不是一个非负数字）
   6. 当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%，如下是等同的：
   7. 当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1
6. `align-self`: 允许单个项目有与其他项目不一样的对齐方式
