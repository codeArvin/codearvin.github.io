---
title: 《学习JavaScript数据结构与算法》读书笔记--链表
date: 2017-09-20 23:05:12
tags: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
keywords: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
categories: 读书笔记
description: 《学习JavaScript数据结构与算法》读书笔记--链表
toc: true
---

## 概念

链表储存有序的元素集合，每个元素由一个储存本身的节点和一个指向下一个元素的引用组成。
有好几种：
- 普通链表：节点包含本身和指向下一个元素的引用
- 双向链表：节点包含本身和指向下一个元素的引用和指向前一个元素的引用
- 循环链表：在普通链表的基础上最后一个节点的下一个元素指向第一个节点
- 双向循环链表：在双向链表的基础上最后一个节点的下一个元素指向第一个节点、第一个节点的前一个元素指向最后一个节点

## 简单代码实现
普通链表
```javascript
function LinkedList() {
  // 节点
  var Node = function(element) {
    this.element = element;
    this.next = null;
  }

  var length = 0;
  var head = null;
  // 顺序添加一个节点
  this.append = function(element) {
    var node = new Node(element);
    var current;

    if (head === null) {
      head = node;
    } else {
      current = head;
      while (current.next) {
        current = current.next;
      }
      current.next = node;
    }

    length ++;
  }
  // 在指定位置添加一个节点
  this.insert = function(position, element) {
    if (position >= 0 && position <= length) {
      var node = new Node(element);
      var current;
      var previous;
      var index = 0;

      if (position === 0) {
        node.next = head;
        head = node;
      } else {
        current = head;
        while (index++ < position) {
          previous = current;
          current = current.next;
        }
        previous.next = node;
        node.next = current;
      }
      length++;
      return node.element;

    } else {
      return false;
    }
  }
  // 返回某个元素的位置
  this.indexOf = function(element) {
    var current = head;
    var index = 0;
    while (current) {
      if (current.element === element) {
        return index;
      }
      index++;
      current = current.next;
    }
    return -1;
  }
  // 删除某个位置的节点
  this.removeAt = function(position) {
    var current = head,
        previous,
        index = 0;

    if (position >= 0 && position < length) {
      if (position === 0) {
        head = head.next;
      } else {
        while (index++ < position) {
          previous = current;
          current = current.next;
        }
        previous.next = current.next;
      }
      length--;
      return current.element;
    } else {
      return false;
    }
  }
  // 移除某个元素
  this.remove = function(element) {
    var index = this.indexOf(element);
    return this.removeAt(index);
  }
  // 链表是否为空
  this.isEmpty = function() {
    return length === 0 ? true : false;
  }
  // 链表的长度
  this.size = function() {
    return length;
  }
  // 返回链表的字符串
  this.toString = function() {
    var current = head;
    var string = '';

    while (current) {
      string += current.element.toString() + ' --> ';
      current = current.next;
    }

    return string === '' ? '' : string.slice(0, string.length - 5);
  }
  // 清空链表
  this.clear = function() {
    head = null;
    length = 0;
  }
  // 返回head的引用
  this.getHead = function() {
    return head;
  }

}
```

其他三种链表和普通链表的处理方式大体相同
1. 双向链表：添加和移除元素的时候需要考虑添加或删除那个元素与前后两个元素的关联关系
2. 循环链表：添加和删除的时候还需要考虑head和最后一个元素的关联
3. 双向循环链表：添加和删除的时候还需要考虑head和最后一个元素的关联
