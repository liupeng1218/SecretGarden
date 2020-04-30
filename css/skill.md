<!-- TOC -->

- [1. 要点](#1-%e8%a6%81%e7%82%b9)
  - [1.1. 负边距](#11-%e8%b4%9f%e8%be%b9%e8%b7%9d)
  - [1.2. flex-margin](#12-flex-margin)
  - [1.3. 定宽高比](#13-%e5%ae%9a%e5%ae%bd%e9%ab%98%e6%af%94)
  - [1.4. 隐藏文本](#14-%e9%9a%90%e8%97%8f%e6%96%87%e6%9c%ac)
  - [1.5. background-position 百分比](#15-background-position-%e7%99%be%e5%88%86%e6%af%94)
  - [1.6. outline](#16-outline)
  - [1.7. object-fit](#17-object-fit)
  - [1.8. 使用 attr()抓取 data-\*](#18-%e4%bd%bf%e7%94%a8-attr%e6%8a%93%e5%8f%96-data)
  - [1.9. 利用 + 美化表单](#19-%e5%88%a9%e7%94%a8--%e7%be%8e%e5%8c%96%e8%a1%a8%e5%8d%95)
- [2. 特效](#2-%e7%89%b9%e6%95%88)
  - [2.1. 渐变绘制彩带](#21-%e6%b8%90%e5%8f%98%e7%bb%98%e5%88%b6%e5%bd%a9%e5%b8%a6)
  - [2.2. 渐变绘制主题墙](#22-%e6%b8%90%e5%8f%98%e7%bb%98%e5%88%b6%e4%b8%bb%e9%a2%98%e5%a2%99)
  - [2.3. 角向渐变](#23-%e8%a7%92%e5%90%91%e6%b8%90%e5%8f%98)
  - [2.4. 动画暂停](#24-%e5%8a%a8%e7%94%bb%e6%9a%82%e5%81%9c)
  - [2.5. 滤镜](#25-%e6%bb%a4%e9%95%9c)
  - [2.6. 加载球](#26-%e5%8a%a0%e8%bd%bd%e7%90%83)

<!-- /TOC -->

# 1. 要点

记录 css 重点知识点，[线上 demo](https://codepen.io/liupeng1991/pen/KKdqaoe)

## 1.1. 负边距

负边距左为负，为左移，右为负时，是左拉。上下和左右类似。

## 1.2. flex-margin

为 flex 的子项设置 `margin:auto` 会实现平均分布的效果
为 flex 的中间项设置 `margin:auto` 会实现两端对齐分布的效果
为 flex 的子项设置 `margin-left:auto` 会将子项移至末端
为 flex 的第一个子项设置 `margin-left:auto`，最后一个子项设置 `margin-right:auto` 会将所有子项居中

## 1.3. 定宽高比

padding 的百分比是相对于其包含块的宽度，而不是高度

## 1.4. 隐藏文本

- `font-size`设置字体大小为 0
- `text-indent` 设置文本缩进值

## 1.5. background-position 百分比

图片自身的百分比位置与容器同样的百分比位置重合

## 1.6. outline

使用 `outline` 来描边，不占用盒子空间。可以设置`outline-offset` 偏移至内侧

## 1.7. object-fit

图片指定尺寸后，可以使用`object-fit`保持比例

## 1.8. 使用 attr()抓取 data-\*

在标签上自定义属性 `data-\*`，通过 `attr()`获取其内容赋值到 `content` 上

## 1.9. 利用 + 美化表单

`<label>`使用 `+` 配合 `for` 绑定 `radio` 或 `checkbox` 的选择行为

# 2. 特效

记录 css 常用效果，[线上 demo](https://codepen.io/liupeng1991/pen/rNOwjbE)

## 2.1. 渐变绘制彩带

利用背景色的 `linear-gradient` 属性 ，绘制渐变效果

## 2.2. 渐变绘制主题墙

利用背景色的 `linear-gradient` 配合 `animation` 属性 ，绘制主题墙效果

## 2.3. 角向渐变

利用背景色 `conic-gradient` 角向渐变实现饼图

## 2.4. 动画暂停

`animation-play-state` 可以控制动画暂停

## 2.5. 滤镜

`filter` 可以使用多种函数直接设置滤镜效果

## 2.6. 加载球

两个不规则球体 配合 `animation` 实现波浪效果
