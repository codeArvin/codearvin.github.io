---
title: transition transform animation
date: 2017-03-22 21:03:13
tags: [IFE, 前端, CSS]
keywords: IFE, 前端, CSS
categories: 学习记录
description: 对CSS中transition transform animation几个属性的理解
toc: true
---

## transition
CSS3的过渡属性，从某一种样式转变到另一种样式时候会产生过渡效果
1、`transition`: `property duration timing-function delay`
  * `property`: 需要过渡的CSS属性。多个属性用`,`隔开
  * `duration`: 过渡时间，单位ms，默认0
  * `timing-function`: 效果的转速曲线。默认ease
    * `linear`: 相同速度
    * `ease`: 慢-快-慢
    * `ease-in`: 慢-正常
    * `ease-out`: 正常-慢
    * `ease-in-out`: 慢-正常-慢
  * `delay`: 延迟多久开始动画，单位ms，默认0

## animation
CSS3的动画属性，可以设置样式的连续变化
通过`@keyframes`设置动画，`animation`设置动画属性
1、`@keyframes`: `animationname {keyframes-selector {css-styles;}}`
示例:
```CSS
/* -- 如果只有两个变化可以用from to 来代替0% 100% --*/
/* -- 多个属性用`;`隔开 -- */
@keyframes mymove
{
0%   {top:0px;}
25%  {top:200px;}
50%  {top:100px;}
75%  {top:200px;}
100% {top:0px;}
}
```

2、`animation`: `name duration timing-function delay iteratin-count direction fill-mode`
`play-state`
  * `name`: 就是`@keyframes`设置的名字
  * `duration`: 动画完成时间
  * `timing-function`: 动画效果的转速曲线，值和`transition`的相同
  * `delay`: 延迟时间
  * `iteration-count`: 动画播放的次数
  * `direction`: 是否循环交替反向播放
    * `normal`: 默认值，正常播放
    * `reverse`: 反向播放
    * `alternate`: 奇次正向，偶次反向
    * `alternate`: 奇次反向，偶次正向
  * `fill-mode`: 动画不播放时的应用样式
  * `play-state`: 指定动画的暂停与播放
    * `paused`: 暂停
    * `running`: 播放

## transform
CSS3的2D、3D转换属性，指定元素的移动翻转拉伸等
**2D**:
1、`translate(x,y)`: 感觉像relative。左上角a,右上角b,左下角c。a为坐标原地，a->b为x轴,a->c为y轴，通过传入x,y值指定位置
2、`rotate(deg)`: 绕元素中心旋转，传入一个角度
3、`scale(x,y)`: 宽变为原来的x倍，高变为原来的y倍
4、`skew(x,y)`: 指定X轴Y轴的倾斜角度
5、`matrix()`: 将2D变换方法合并成一个
**3D**:
1、`rotateX(deg)`: 以X轴旋转
2、`rotateY(deg)`: 以Y轴旋转

**先写这么多，后续再完善**

## 渐变
`background`: `linear-gradient(direction, color-stop1, color-stop2, ...)`
`direction`: 渐变的方向，可以取以下这几种值: `left`、`left top`、`0deg`、`90deg`
