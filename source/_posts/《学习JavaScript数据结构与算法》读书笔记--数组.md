---
title: 《学习JavaScript数据结构与算法》读书笔记--数组
date: 2017-03-25 16:05:23
tags: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
keywords: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
categories: 读书笔记
description: 《学习JavaScript数据结构与算法》读书笔记--数组
toc: true
---
# 方法

## concat
`array1.concat(array2, array3, ...)`:
  * 作用: 用于连接两个或多个数组。
  * 参数: 多个数组
  * 返回值: 连接后的新数组
  * 注意: **不会改变现有的数组**

## every
`array.every(function(currentValue, index, arr), thisValue)`:
  * 作用: 检测数组中所有元素是否都符合指定条件
  * 参数: 函数、 `thisValue`: 传入的`this`值，可选
  * 返回值: true OR false
  * 注意:
    * 检测到不满足条件的元素时，返回false，且剩余元素不再执行检测
    * 不对空数组进行检测
    * 不改变原数组

## some
`array.some(function(currentValue, index, arr), thisValue)`:
  * 作用: 检测数组中是否有元素符合条件，有一个满足则返回true
  * 参数: 函数
  * 返回值: true OR false
  * 注意: 不会对空数组进行检测， 不会改变原数组

## fill
`array.fill(value, start, end)`:
  * 作用: 用于将一个固定值替换数组中的元素
  * 参数:
    * value: **必需**， 填充的值
    * start: **可选**，开始填充位置(包括)
    * end: **可选**， 停止填充位置(不包括)，默认为`array.length`
  * 返回值: 填充后的数组
  * 注意: 改变原数组

## filter
`array.filter(function(currentValue, index, arr), thisValue)`:
  * 作用: 返回符合过滤条件的数组元素
  * 参数: 函数、`thisValue`: 传入的`this`值，可选
  * 返回值: 由符合条件元素组成的新数组
  * 注意:
    * 不会对空数组进行检测
    * 不会改变原有数组

## find
`array.find(function(currentValue, index, arr), thisValue)`:
  * 作用: 返回符合传入函数条件的数组第一个元素
  * 参数: 函数、`thisValue`: 传入的`this`值，可选
  * 返回值: 符合条件的数组元素，没有符合条件返回-1
  * 注意:
    * 不会对空数组进行检测
    * 不会改变原数组

## findIndex
`array.findIndex(function(currentValue, index, arr), thisValue)`: 作用和`find()`相同，只不过返回的是元素的索引值

## forEach
`array.forEach(function(currentValue, index, arr), thisValue)`:
  * 作用: 对数组每个元素执行某项操作
  * 参数: 函数
  * 返回值: `undefined`
  * 注意:
    * 不改变原数组
    * 空数组不执行

## map
`array.map(function(currentValue, index, arr), thisValue)`:
  * 作用: 对数组每个元素执行某种操作，将处理后的值以新数组的形式返回
  * 参数: 函数
  * 返回值: 新数组，数组中的元素为原始数组元素调用函数处理后返回的值
  * 注意:
    * 按照原始数组元素顺序依次处理元素
    * 不会对空数组进行执行
    * 不改变原数组

## indexOf
`array.indexOf(item, start)`:
  * 作用: 返回查找字符串在数组首次出现的位置
  * 参数:
    * item: 要查找的元素
    * start: 在数组中开始检索的位置，未指定从0开始
  * 返回值: 元素在数组中的位置，未找到返回-1

## lastIndexOf
`array.lastIndexOf(item, start)`: 作用和`indexOf()`相同，不过返回的是元素最后一次出现的位置，start则是从该位置向前查找

## join
`array.join(separator)`:
  * 作用: 把数组中的所有元素转换为一个字符串
  * 参数: 可选，指定要选择的分隔符，如省略以逗号作为分隔符
  * 返回值: 生成的字符串

## shift
`array.shift()`:
  * 作用: 删除数组第一个元素并返回该元素
  * 参数: 无
  * 返回值: 被删除的元素
  * 注意: 改变原数组

## pop
`array.pop()`:
  * 作用: 删除数组最后一个元素并返回
  * 参数: 无
  * 返回值: 被删除的元素
  * 注意: 改变原数组及其长度

## unshift
`array.unshift(item1, item2, ...)`:
  * 作用: 向数字开头添加一个或多个元素，并返回新的长度
  * 参数: 要添加的元素
  * 返回值: 新数组的长度
  * 注意: 改变原数组

## push
`array.push(item1, item2, ...)`:
  * 作用: 向元素末尾添加一个或多个元素，并返回新的长度
  * 参数: 一个或多个元素
  * 返回值: 插入新元素后的数组长度
  * 注意: 改变原数组及其长度

## reduce
`array.reduce(function(total, currentValue, currentIndex, arr), initialValue)`:
  * 作用: 接收一个函数作为累加器，数组中的每一个值(从左到右)开始缩减，最终计算为一个值
  * 参数:
    * total: 必需，初始值或者计算结束的返回值
    * currentValue: 必需，当前元素
    * currentIndex: 可选，当前元素的索引
    * arr: 可选，当前的数组对象
    * initialValue: 可选，传递给函数的初始值
  * 返回值: 返回计算的结果
  * 注意: 不改变原数组，对空数组不执行

## reduceRight
`arr.reduceRight(function(total, currentValue, currentIndex, arr), initialValue)`: 作用和`array.reduce()`相同，不过累加的方向是从数组末尾开始

## reverse
`arr.reverse()`:
  * 作用: 用于颠倒数组中元素的方向
  * 参数: 无
  * 返回值: 颠倒后的数组
  * 注意: 改变原数组

## slice
`array.slice(start, end)`:
  * 作用: 从数组中返回选定范围的元素
  * 参数:
    * start: 必须，范围的开始位置(包含)，如果是负数从尾部开始算起，-1指最后一个元素
    * end: 可选，范围的结束位置(不包含)，不指定则截取到数组末尾。如果为负数从尾部算起，-1指最后一个元素
  * 返回值: 指定范围的数组元素
  * 注意: 不改变原有数组

## splice
`array.splice(index, howmany, item...)`:
  * 作用: 用于插入删除或替换数组的元素
  * 参数:
    * index: 从index处(包含)开始添加/删除元素
    * howmany: 应该删除多少元素，为0表示不删除元素，未规定则删除从index开始到原数组结尾所有元素
    * item...: 可选，要添加的元素
  * 返回值: 如果删除了元素，返回的是含有被删除元素的数组。没有删除元素返回空数组

## sort
`array.sort(sortfunction(a, b))`:
  * 作用: 对数组的元素进行排序，默认为按字母升序
    * 如果函数返回值小于0，a会被排到b之前
    * 如果函数返回值等于0，a和b的相对位置不变
    * 如果函数返回值大于0，a会被排到b之后
  * 参数: 排序函数
  * 返回值: 对数组的引用
  * 注意: 数组在原数组上进行排序，不生成副本。即会改变原数组
  * PS: 为啥我感觉这个传入的函数好难理解.我就理解成返回值小于0，顺序就是形参的位置顺序

## toString
`array.toString()`: 将数组元素转化为字符串，元素间用逗号隔开

# 总结
* 改变原数组的方法: `fill`、`shift`、`pop`、`push`、`unshift`、`reverse`、`splice`
