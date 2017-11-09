---
title: IFE糯米学院--有趣的鼠标悬浮模糊效果的实现
date: 2017-03-22 22:56:52
tags: [IFE, 前端, CSS]
keywords: IFE, 前端, CSS
categories: 学习记录
description: 鼠标悬浮模糊效果的实现和相关知识点的记录
toc: true
---
## 需要实现的功能
1、鼠标悬浮时背景图片产生模糊效果
2、实现文字的流光渐变效果
3、按钮边框从中间到两边扩展开

## 实现思路
1、利用CSS3的filter属性，blur()实现
2、把文字看成纸上的镂空，背景是丰富的颜色，背景在动，就会实现这种效果。为此需要一下几个CSS属性：
  * `background: linear-gradient(direction, color-stop1, color-stop2, ...)`: 实现背景的颜色渐变
  * `color: transparent`: 文字颜色透明，可以看到背景
  * `background-clip: text`: 裁剪背景只在文字区域显示
  * `background-size: 200% 100%`: 背景区域变宽，通过`background-position`移动背景

3、我添加了两个div，分别设置了上下和左右边框，然后以中心为轴旋转90deg，这样就看不到了。鼠标悬浮的时候再旋转回来，视觉效果上就是边框从中心向两边伸展开来
4、细节上采用了`transition`来保证鼠标移入和移出时的过渡动画。最开始都采用的`animation`实现动画，但发现鼠标移出的时候消失的很生硬，就切换成了`transtion`来实现过渡

## 部分代码
```CSS
/* -- 图片模糊效果 -- */
#image {
  -webkit-filter: blur(4px);
  filter: blur(4px);
}

/* -- 文字的流光渐变 -- */
#text {
  color: transparent;
  background: -webkit-linear-gradient(left, #147B96, #E6D205 25%, #147B96 50%, #E6D205 75%, #147B96);
  background: -o-linear-gradient(left, #147B96, #E6D205 25%, #147B96 50%, #E6D205 75%, #147B96);
  background: -moz-linear-gradient(left, #147B96, #E6D205 25%, #147B96 50%, #E6D205 75%, #147B96);
  background: linear-gradient(left, #147B96, #E6D205 25%, #147B96 50%, #E6D205 75%, #147B96);
  -webkit-background-clip: text;
  -ms-background-clip: text;
  -moz-background-clip: text;
  background-clip: text;
  background-size: 200% 100%;
  animation: textGradual 2s infinite linear;
}

@keyframes textGradual {
  from {background-position: 0 0}
  to {background-position: -100% 0}
}
```

## 完整代码和SHOW
[完整代码](https://github.com/codeArvin/IFE/tree/master/nuomi/task1)有点乱
[SHOW](https://codearvin.github.io/IFE/nuomi/task1/index.html)
