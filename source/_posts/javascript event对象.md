---
title: javascript event对象
date: 2017-03-23 20:46:45
tags: [IFE, 前端, JS]
keywords: IFE, 前端, JS
categories: 学习记录
description: 对JavaScript中event对象属性的解释
toc: true
---

## clientX几个相似属性
1、`event.clientX`、`event.clientY`:
鼠标相对于浏览器窗口可视区域的X，Y坐标（窗口坐标），可视区域不包括工具栏和滚动条。IE事件和标准事件都定义了这2个属性
2、`event.pageX`、`event.pageY`:
类似于event.clientX、event.clientY，但它们使用的是文档坐标而非窗口坐标。这2个属性不是标准属性，但得到了广泛支持。IE事件中没有这2个属性。
3、`event.offsetX`、`event.offsetY`:
鼠标相对于事件源元素（srcElement）的X,Y坐标，只有IE事件有这2个属性，标准事件没有对应的属性。
4、`event.screenX`、`event.screenY`:
鼠标相对于用户显示器屏幕左上角的X,Y坐标。标准事件和IE事件都定义了这2个属性

**图示**:
![图片来源红黑联盟](javascript event对象/clientX.png)
