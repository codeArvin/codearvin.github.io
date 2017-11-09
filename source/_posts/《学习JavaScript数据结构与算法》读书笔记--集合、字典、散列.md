---
title: 《学习JavaScript数据结构与算法》读书笔记--集合、字典、散列
date: 2017-10-09 18:55:12
tags: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
keywords: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
categories: 读书笔记
description: 《学习JavaScript数据结构与算法》读书笔记--集合、字典、散列
toc: true
---

# 集合

## 概念

**集合**是由一组无序且唯一的项组成

## 简单代码实现
```javascript
function Set() {
  let items = {};
  // 向集合添加一个新的项
  this.add = (value) => {
    if (!this.has(value)) {
      items[value] = value;
      return true;
    }
    return false;
  }
  // 从集合移除一个值
  this.remove = (value) => {
    if (this.has(value)) {
      delete items[value];
      return true;
    }
    return false;
  }
  // 值是否存在于集合中
  this.has = (value) => items.hasOwnProperty(value)
  // 清空集合
  this.clear = () => { items = {} }
  // 返回集合元素个数
  this.size = () => Object.keys(items).length
  // 返回包含集合所有元素的数组,这里要用Object.values，因为keys只能是字符串
  this.values = () => Object.values(items)
}
```

## ES6中的 Set

ES6中的 Set 结构就是集合的实现

1. 基本用法：
  * Set 函数可以接受一个数组(或者具有 iterable 接口的其他数据结构)作为参数，用来初始化
  * 向Set中添加值的时候不会发生类型转换
  * Set内部判断两个值是否不同使用的算法叫做`Same-value equality`，类似于精确相等运算符(`===`)，**主要的区别是`NaN`等于自身**，而`===`认为`NaN`不等于自身，另外，两个对象总是不相等的
2. 属性和方法
  * `Set.prototype.constructor`：构造函数，默认就是`Set`函数
  * `Set.prototype.size`：返回`Set`实例的元素总数
  * `add(value)`: 添加值，返回`Set`本身
  * `delete(value)`: 删除某个值，返回一个布尔值表示删除是否成功
  * `has(value)`: 返回一个布尔值表示该值是否为Set的成员
  * `clear()`: 清除所有成员，没有返回值
  * `keys()`: 返回键名的遍历器
  * `values()`: 返回键值的遍历器
  * `entries()`: 返回键值对的遍历器
  * `forEach()`: 使用回调函数遍历每个成员，遍历顺序就是插入顺序。
3. 遍历的应用
```javascript
// 数组去重
[...new Set([1,1,1,2,2,2])]
// 将Set转换为数组使用map和filter
let set = new Set([1,2,3]);
set = new Set([...set].map(x => x * 2))
set = new Set([...set].filter( x => (x % 2) !== 0))
// 实现并集、交集、差集
let a = new Set([1,2,3]);
let b = new Set([2,3,4]);
let union = new Set([...a, ...b]);
let intersect = new Set([...a].filter(x => b.has(x)));
let difference = new Set([...a].filter(x => !b.has(x)));
```

# 字典

## 概念

**字典**也称映射，以[键，值]的形式来储存元素

## 简单代码实现
```javascript
function Dictionary() {
  let items = {};
  // 添加新元素
  this.set = function(key, value) {
    items[key] = value;
  }
  // 使用键名移除对应数据
  this.remove = function(key) {
    if (this.has(key)) {
      delete items[key];
      return true;
    }
    return false;
  }
  // 返回一个布尔值表示某个键名是否存在
  this.has = key => items.hasOwnProperty(key)
  // 通过键名返回对应的值
  this.get = key => items[key] ? items[key] : undefined
  // 清空所有元素
  this.clear = () => { items = {} }
  // 返回字典包含元素的个数
  this.size = () => Object.keys(items).length
  // 返回键名的遍历器
  this.keys = () => Object.keys(items)
  // 返回键值的遍历器
  this.values = () => Object.values(items)
}
```

## ES6 中的 Map

ES6 中的 Map 结构就是字典的实现

1. 基本用法
  * Map 与对象的不同是对象只接受字符串作为键名，而Map的键名可以是任何形式
  * Map函数接受一个数组(或者任何具有iterable接口，且每个元素都是一个双元素的数组的数据结构)作为参数来初始化
2. 属性和方法
 * `size`: 返回Map结构的成员总数
 * `set(key, value)`: 设置键名`key`对应的键值为`value`，返回整个Map结构，如果`key`值已经存在，则更新
 * `get(key)`: 返回`key`对应的键值
 * `has(key)`: 返回一个布尔值表示该键名是否存在于当前Map对象中
 * `delete(key)`: 删除某个键，返回布尔值
 * `clear()`: 清除所有成员，没有返回值
 * `keys()`: 返回键名的遍历器
 * `values()`: 返回键值的遍历器
 * `entries()`: 返回所有成员的遍历器
 * `forEach()`: 遍历Map的所有成员，遍历顺序就是插入顺序

# 散列

## 概念

像数组一类的数据结构获取其中的某个值我们需要遍历整个数据结构来找到它。散列表则是通过散列函数直接获得值的位置从而获取到值

## 简单代码实现
```javascript
const loseloseHashCode = key => {
  let hash = 0;
  for (let i = 0; i < key.length; i++) {
    hash += key.charCodeAt(i);
  }
  return hash % 37;
}
function HashTable() {
  let table = [];
  this.put = (key, value) => table[loseloseHashCode(key)] = value;
  this.get = key => table[loseloseHashCode(key)]
  this.remove = key => table[loseloseHashCode(key)] = undefined
}
```

## 相关

1. 由于不同的键名通过散列函数可能会得到相同的值从而产生碰撞，称为冲突。解决冲突的方法主要有这几种：分离链接、线性探查、双散列法
  * 分离链接：在散列表的每个位置创建一个链表并将元素储存在里面，但是它在HashTable实例外还需要额外的储存空间
  ```javascript
  function ValuePair(key, value) {
    this.key = key;
    this.value = value;
  }
  function HashTable() {
    let table = [];
    this.put = (key, value) => {
      const position = loseloseHashCode(key);
      if (table[position] === undefined) {
        table[position] = new LinkedList();
      }
      table[position].append(new ValuePair(key, value));
    }
    this.get = key => {
      const position = loseloseHashCode(key);
      if (table[position] !== undefined) {
        let current = table[position].getHead();
        while (current.next) {
          if (current.next.element.key === key) {
            return current.next.element.value;
          }
          current = current.next;
        }
        if (current.element.key === key) {
          return current.element.value;
        }
      }
      return undefined;
    }
    this.remove = key => {
      const position = loseloseHashCode(key);
      if (table[position] !== undefined) {
        let current = table[position].getHead();
        while (current.next) {
          if (current.next.element.key === key) {
            table[position].remove(current.next.element);
            return true;
          }
          current = current.next;
        }
        if (current.element.key === key) {
          table[position].remove(current.element);
          // 当没有元素的时候，需要把该位置设为undefined
          if (table[position].isEmpty()) {
            table[position] = undefined;
          }
          return true;
        }
      }
      return false;
    }
  }
  ```
  * 线性探查：当想向表中某个位置添加一个新元素的时候，如果索引为index的位置已经被占用了，就尝试index+1的位置，如果依旧被占用，就尝试index+2的位置，依此类推
  ```javascript
  function ValuePair(key, value) {
    this.key = key;
    this.value = value;
  }
  function HashTable() {
    let table = [];
    this.put = (key, value) => {
      let index = loseloseHashCode(key);
      while (table[index] !== undefined) {
        index++;
      }
      table[index] = new ValuePair(key, value);
    }
    this.get = key => {
      let index = loseloseHashCode(key);
      if (table[index] !== undefined) {
        while(table[index] !== undefined && table[index].key !== key) {
          index++;
        }
        if (table[index].key === key) {
          return table[index].value;
        }
        return undefined;
      }
      return undefined;
    }
    this.remove = key => {
      let index = loseloseHashCode(key);
      if (table[index] !== undefined) {
        while (table[index] !== undefined && table[index].key !== key) {
          index++;
        }
        if (table[index].key === key) {
          table[index] = undefined;
          return true;
        }
        return false;
      }
      return false;
    }
  }
  ```
