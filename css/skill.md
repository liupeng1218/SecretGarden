<!-- TOC -->

- [要点](#要点)
  - [负边距](#负边距)
  - [flex-margin](#flex-margin)
  - [定宽高比](#定宽高比)
  - [隐藏文本](#隐藏文本)
  - [background-position 百分比](#background-position-百分比)
  - [outline](#outline)
  - [object-fit](#object-fit)
  - [使用 attr()抓取 data-\*](#使用-attr抓取-data-\)
  - [利用 + 美化表单](#利用--美化表单)
- [特效](#特效)
  - [渐变绘制彩带](#渐变绘制彩带)
  - [渐变绘制主题墙](#渐变绘制主题墙)
  - [角向渐变](#角向渐变)
  - [动画暂停](#动画暂停)
  - [滤镜](#滤镜)
  - [加载球](#加载球)

<!-- /TOC -->

# 要点

记录 css 重点知识点，[线上 demo](https://codepen.io/liupeng1991/pen/KKdqaoe)

## 负边距

负边距左为负，为左移，右为负时，是左拉。上下和左右类似。

## flex-margin

为 flex 的子项设置 `margin:auto` 会实现平均分布的效果
为 flex 的中间项设置 `margin:auto` 会实现两端对齐分布的效果
为 flex 的子项设置 `margin-left:auto` 会将子项移至末端
为 flex 的第一个子项设置 `margin-left:auto`，最后一个子项设置 `margin-right:auto` 会将所有子项居中

## 定宽高比

padding 的百分比是相对于其包含块的宽度，而不是高度

## 隐藏文本

- `font-size`设置字体大小为 0
- `text-indent` 设置文本缩进值

## background-position 百分比

图片自身的百分比位置与容器同样的百分比位置重合

## outline

使用 `outline` 来描边，不占用盒子空间。可以设置`outline-offset` 偏移至内侧

## object-fit

图片指定尺寸后，可以使用`object-fit`保持比例

## 使用 attr()抓取 data-\*

在标签上自定义属性 `data-\*`，通过 `attr()`获取其内容赋值到 `content` 上

## 利用 + 美化表单

`<label>`使用 `+` 配合 `for` 绑定 `radio` 或 `checkbox` 的选择行为

# 特效

记录 css 常用效果，[线上 demo](https://codepen.io/liupeng1991/pen/rNOwjbE)

## 渐变绘制彩带

利用背景色的 `linear-gradient` 属性 ，绘制渐变效果

## 渐变绘制主题墙

利用背景色的 `linear-gradient` 配合 `animation` 属性 ，绘制主题墙效果

## 角向渐变

利用背景色 `conic-gradient` 角向渐变实现饼图

## 动画暂停

`animation-play-state` 可以控制动画暂停

## 滤镜

`filter` 可以使用多种函数直接设置滤镜效果

## 加载球

两个不规则球体 配合 `animation` 实现波浪效果
