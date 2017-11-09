---
title: JavaScript操作DOM常用API总结
date: 2017-03-28 13:52:23
tags: [前端, DOM, JS]
keywords: [前端, DOM, JS]
categories: 学习记录
description: JavaScript操作DOM常用API
toc: true
---

原文地址:[http://www.imooc.com/article/2562](http://www.imooc.com/article/2562)

## 基本概念
1、Node类型:
  * JS中所有节点都继承自Node类型，都共享着相同的基本属性和方法
  * Node有一个属性`nodeType`表示Node的类型，是一个整数
  * 我们最常用的就是`element`、`text`、`attribute`、`comment`、`document`、`document_fragment`

2、`HTMLCollection`和`NodeList`的区别:
  * `HTMLCollection`是元素集合，`NodeList`是节点集合(即可以包含元素，也可以包含文本节点)
  * 相同点:
    * 都是类数组对象，有length属性
    * 都是只读的
    * 都是实时的，有个例外就是`querySelectorAll`返回的NodeList不是实时的
    * 都有item()方法，通过index或id获取元素
    * 都可以通过属性的方式访问元素
  * 不同点:
    * `HTMLCollection`对象有namedItem()方法，可以传递id或name来获取元素
    * `HTMLCollection`的item方法还有通过属性获取元素可以支持id和name，而`NodeList`只支持id

## API

### 节点创建型API

1、`document.createElement(tagName)`
2、`document.createTextNode(text)`
3、`node.cloneNode(bool)`: 接收一个boll参数，表示是否复制子元素
  * bool值最好传入
  * 通过`addEventListener`或`onclick`进行绑定，克隆副本不会绑定该事件，如果是内联绑定克隆节点会绑定该事件
4、`document.createDocumentFragment()`: 创建一个`documentFragment`，作用主要用于储存临时的节点准备添加到文档中

总结:
  * 创建的节点是孤立的，要通过appendChild等方法添加到文档中
  * cloneNode要注意被复制的节点是否包含子节点和事件绑定问题
  * createDocumentFragment用来解决添加大量节点时的性能问题

### 页面修改型API

1、`parent.appendChild(child)`:
  * 添加节点会作为父节点的最后一个子节点
  * 如果被添加的节点在页面中存在，则执行后这个节点会添加到指定位置，原本位置会移除该节点

2、`parent.insertBefore(newNode, child)`: 如果第二个参数为`undefined`或`null`则会添加到末尾
3、`parent.removeChild(node)`
4、`parent.replaceChild(newChild, oldChild)`

### 节点查询型API

1、`document.getElementById(id)`: 只查找存在于文档中的元素，如果有多个元素id相同，返回第一个
2、`document.getElementsByTagName(tagName)`:
  * 返回一个即时的HTMLCollection类型,即该集合内部的元素会随着你添加删除等操作随时变化
  * 如果没有找到，返回一个空的HTMLCollection
  * `*`表示所有标签

3、`document.getElementsByName`:
  * 返回一个即时的Nodelist对象，随时变化
  * html中并不是所有元素都有name属性，比如div，但如果设置也是可以被查到的
  * 在IE中，如果id设置成某个值，传入该函数的参数与id值相同，也可以被找到

4、`document.getElementsByClassName(classNames)`:
  * 返回一个即时的HTMLCollection
  * 如果获取两个以上的classname，可以传入多个，用空格分隔

5、`document.querySelector`:
  * 通过css选择器查找元素，返回第一个匹配的元素，没有匹配返回null。
  * 算法使用的是**深度优先搜索**来获取元素

6、`document.querySelector`:
  * 和上面方法一样，不同的是返回所有匹配的元素，可以匹配多个选择符
  * 搜索方式也是深度优先搜索。
  * 返回的是非即时的NodeList

### 节点关系型API

1、父关系型:
  * parentNode: 表示元素的父节点
  * parentElement: 返回元素的父元素节点，父节点如果不是element，返回null

2、兄弟关系型:
  * previousSibling: 节点的前一个节点
    * 如果该节点是第一个节点，返回null
    * 注意有可能拿到的是文本节点或注释节点，要进行处理
  * previousElementSibling: 返回前一个**元素**节点
  * nextSibling: 节点的后一个节点
    * 如果该节点是最后一个节点，返回null
    * 注意有可能拿到的是文本节点或注释节点，要进行处理
  * nextElementSibling: 返回一个元素节点

3、子关系型:
  * childNodes: 返回一个即时的NodeList，表示子元素的子节点列表
  * children: 一个即时的HTMLCollection，子节点都是element
  * firstNode: 第一个子节点
  * lastNode: 最后一个子节点
  * hasChildNodes: 判断是否包含子节点

4、元素属性型:
  * element.setAttribute(name, value): 根据名称和值修改元素的特性
  * element.getAttribute(name): 返回特性名相应的值，没有返回null或空字符串

5、元素样式型:
  * window.getComputedStyle(element, pseudoElt):
    * 获得元素计算后的样式
    * 返回一个CSSStyleDecaration对象
  * element.getBoundingClientRect():
    * clientRect是一个DOMRect对象，包含`left`、`top`、`bottom`、`right`，是相对于可视窗口的距离，滚动距离发生改变时，他们的值会发生改变
