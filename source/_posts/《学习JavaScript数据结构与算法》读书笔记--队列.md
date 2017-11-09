---
title: 《学习JavaScript数据结构与算法》读书笔记--队列
date: 2017-03-29 18:55:12
tags: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
keywords: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
categories: 读书笔记
description: 《学习JavaScript数据结构与算法》读书笔记--队列
toc: true
---

## 概念

队列和栈很相似，不过队列遵循的是先进先出(FIFO)的原则。队列在尾部添加新元素，并从顶部移除元素。新添加的元素必须排在队列的末尾

**优先队列**: 元素的添加和移除是基于优先级的。有两种实现方式
  * 设置优先级，在正确的位置添加元素
  * 用正常的入列操作添加元素，按照优先级进行移除

## 简单代码实现
传统队列
```javascript
var Queue = function () {
  // 存放队列元素
  var items = [];

  // 向队列末尾添加一个元素
  this.enqueue = function(element) {
    items.push(element);
  };

  // 从队列顶部移除一个元素并返回
  this.dequeue = function() {
    return items.shift();
  };

  // 查看队列顶部第一个元素，不改变队列
  this.front = function() {
    return items[0];
  };

  // 队列是否为空
  this.isEmpty = function() {
    return items.length === 0;
  };

  // 返回队列的元素个数
  this.size = function() {
    return items.length;
  };

  // 在控制台打印出队列的字符串形式
  this.print = function() {
    console.log(items.toString());
  };
}
```
优先队列
```JavaScript
var PriorityQueue = function() {
  var items = [];

  var QueueElement = function(element, priority) {
    this.element = element;
    this.priority = priority;
  };

  this.enqueue = function(element, priority) {
    var queueElement = new QueueElement(element, priority);

    if (this.isEmpty()) {
      items.push(queueElement);
    } else {
      var added = false;
      for (var i = 0; i < items.length; i++) {
        // priority越小，优先级越大，优先级相同时按入队先后排序
        if (items[i].priority > queueElement.priority) {
          items.splice(i, 0, queueElement);
          added = true;
          break;
        }
      }
      if (!added) {
        items.push(queueElement);
      }
    }
  };

  // 在控制台打印出队列的字符串形式
  this.print = function() {
    for(var i = 0; i < items.length; i++) {
      console.log(items[i].element + ' -- ' + items[i].priority);
    }
  };

  // 其他方法和Queue一样
}
```

## 应用

### 击鼓传花游戏模拟
1、概念: 在游戏中，孩子们围成一个圆圈，将花尽快的传递给旁边的人，某一时刻传花停止，花在谁的手中谁被淘汰，直至剩下最后一个孩子为胜利者

2、原理: 利用循环队列

3、代码:
```JavaScript
var hotPotato = function(nameList, num) {
  var queue = new Queue();

  for (var i = 0; i < nameList.length; i++) {
    queue.enqueue(nameList[i]);
  }

  var eliminated = '';

  while (queue.size() > 1) {
    for (var i = 0; i < num; i++) {
      queue.enqueue(queue.dequeue());
    }
    eliminated = queue.dequeue();
    console.log(eliminated + '在击鼓传花游戏中被淘汰');
  }

  return queue.dequeue();
};
```
