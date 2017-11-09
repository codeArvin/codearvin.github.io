---
title: IFE学习Tips记录
date: 2017-03-17 21:20:58
tags: [IFE, 前端]
keywords: IFE, 前端
categories: 学习记录
description: 记录了一些在2017IFE前端学习过程中的一些小知识点
toc: true
---
## 正则相关

### 匹配模式
* `\d`: 数字
* `\D`: 非数字
* `\w`: 所有的单词字符（字母、数字、下划线）`[_a-zA-Z0-9]`
* `\W`: 非单词字符`[^a-zA-Z0-9]`
* `\s`: 空白符`[ \t\n\r]`
* `\S`: 非空白符
* `\t`: 制表符
* `\n`: 换行符
* `\t`: 回车符
* `\b`: 单词边界
* `\B`: 非单词边界
* `.`: 匹配任意单字符，除了`\n`
* `*`: 零次或多次
* `+`: 一次或多次
* `?`: 零次或一次
* `?`:放到()第一个选项前可取消缓存
* `[\u4e00-\u9fa5]`匹配中文汉字，`[\u0391-\uFFE5]`匹配全角字符，包括中文符号和中文汉字

### RegExp对象
* `var patt = new RegExp(pattern, modifiers)` OR `var patt = /pattern/modifiers`
* modifiers:
  * `i`: 执行对大小写不敏感的匹配
  * `g`: 执行全局匹配
  * `m`: 执行多行匹配
* `test()`: 搜索字符串指定值，根据结果返回真或假
* `exec()`: 检索字符串中指定值，返回值是一个数组，例如`var regex = /hello/; var str = 'helloworld'; var result = regex.exec(str)//['hello', index: 0, input: 'helloworld'] `，没有匹配则返回`null`。**但这个方法只能返回第一个匹配的值**

### 支持正则的String对象方法
* `search()`: 用于检索字符串中符合正则的子字符串，返回其起始位置，若找不到则返回-1
* `match()`: 检索符合正则的子字符串，返回数组，若正则指定`g`，则包含多个匹配对象，没有匹配则返回null
* `replace(searchValue, newValue)`: 用`newValue`替换`searchValue`，返回生成的新字符串
* `split(separator, limit)`: 以`separator`分割数组，`limit`指定返回数组的最大长度。返回分割后的数组

### `select`对象
* `select.length`: 其下`option`的个数，设为`0`可以清空下拉数组
* `select.options`: 返回其下拉列表所有选项的一个数组
* `add(option, before)`: 添加`option`或`optgroup`元素,`before`则指定添加到哪个元素后面,如果为`null`则添加到选项数组的末尾
* `remove(index)`: 删除下拉列表中的索引值为index的元素

## DOM相关
1. [菜鸟教程DOM参考手册](http://www.runoob.com/jsref/jsref-tutorial.html)

## 布局相关

### `inline-block`间空隙的解决方法
`inline-block`间空隙出现的原因是标签与标签之间的空格也会被浏览器解析。有以下几种方法
1、将两个标签贴合在一起，即前一个标签的结束标签和后一个标签的开始标签挨着写
2、设置负的 `margin`
3、将父元素的`font-size`设为0即可
4、不写结束标签

## CSS相关
1、通过`element.style.transform`获取或设置CSS中的`transform`属性
2、DOM元素原来有方向，`rotate`之后再`translate`会适应元素旋转后的方向。**注意**：**这个效果必须先旋转再平移**
3、`transition`：用来进行CSS变化的过渡，需要指定对应的CSS属性，用逗号分割不同的属性
4、用js获取节点属性
  * 通过`element.style`获取或设置的样式是元素的内联样式而不是css样式。如果样式名含有类似`background-color`等短杠连接，要写成驼峰写法，如`backgroundColor`
  * 如果想获得一个元素最终展示到页面上的样式值可以通过一下方法
    * 非ie浏览器: `window.getComputedStyle(element, null/伪类)`: 第一个参数是节点，第二个参数是null时获取节点本身样式，是伪类(:before, :after etc.)时获取节点伪类样式
    * ie浏览器: `element.currentStyle`

可以写一个通用获取样式的方法
```javascript
var style = element.currentStyle ? element.currentStyle : window.getComputedStyle(element, null);
```
5、`border-collapse: collapse`用来设置边框合并
6、自定义checkbox。
  * 通过CSS伪元素实现。将input隐藏，通过改变相应label样式达到目的
  * 通过雪碧图。改变`background-position`属性达到目的

7、`box-shadow`: `h-shadow v-shadow blur spread color inset`
  * `h-shadow`: 必需，水平阴影位置
  * `v-shadow`: 必需，垂直阴影位置
  * `blur`: 可选，模糊距离
  * `spread`: 可选，阴影大小
  * `color`: 可选，阴影颜色
  * `inset`: 可选，从外层的阴影(开始时)改变阴影内侧阴影

## 其他操作
1、`parseInt()`: 把一个字符串解析成整数
