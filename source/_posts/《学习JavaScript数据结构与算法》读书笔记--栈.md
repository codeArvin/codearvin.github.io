---
title: 《学习JavaScript数据结构与算法》读书笔记--栈
date: 2017-03-28 14:55:23
tags: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
keywords: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
categories: 读书笔记
description: 《学习JavaScript数据结构与算法》读书笔记--栈
toc: true
---

## 概念

栈是一种遵循**后进先出**(LIFO)的有序集合。新添加的或待删除的元素保存在栈的末尾，称为栈顶，另一端称作栈底。在栈中，新元素都靠近栈顶，旧元素靠近栈底

我把栈想象成一个**装可比克的圆柱桶**，下面是栈底，上面是栈顶。

## 简单代码实现
```JavaScript
function Stack() {

    var items = [];

    // 添加一个元素到栈顶
    this.push = function(element){
        items.push(element);
    };

    // 移除栈顶的元素并返回
    this.pop = function(){
        return items.pop();
    };

    // 返回栈顶的元素，不对栈进行修改
    this.peek = function(){
        return items[items.length-1];
    };

    // 栈是否为空
    this.isEmpty = function(){
        return items.length == 0;
    };

    // 返回栈的元素个数
    this.size = function(){
        return items.length;
    };

    // 清空栈
    this.clear = function(){
        items = [];
    };

    // 打印栈
    this.print = function(){
        console.log(items.toString());
    };

    // 返回栈的字符串表示
    this.toString = function(){
        return items.toString();
    };
}
```

## 应用

### 进制转换器(十进制转换成其他进制)
1、概念:
  * 基数: n进制，n为基数
  * 位权: 对于多位数，处在某一位上的1所表示的数值的大小，称为该位的位权

2、原理: 将十进制数与要转换的进制基数相除，直至商为0，将余数从后向前排列即得到转换后的进制数

3、代码:
```JavaScript
// base: 基数; decNumber: 十进制数; rem: 余数
var baseConverter = function(decNumber, base) {
  var remStack = new Stack(),
      rem,
      baseString = '',
      digits = '0123456789ABCDEF';

  while (decNumber > 0) {
    rem = Math.floor(decNumber % base);
    remStack.push(rem);
    decNumber = Math.floor(decNumber / base);
  }

  while (!remStack.isEmpty()) {
    baseString += digits[remStack.pop()];
  }

  return baseString;
};
```

### 平衡圆括号问题
1、概念: {(()())}是平衡的，([]{}()})是不平衡的

2、原理: 从左向右查找时，第一个遇到的右括号，一定与它左侧最近的左括号匹配。同样，最后一个右括号，一定与第一个左括号相匹配。很像入栈出栈操作

3、代码:
```JavaScript
var match = function(open, close) {
  var opens = '([{',
      closes = ')]}';

  return opens.indexOf(open) === closes.indexOf(close);
};

var parenthesesChecker = function(symbols) {
  var stack = new Stack(),
      balanced = true,
      index = 0,
      len = symbols.length,
      symbol, top;

  while (index < len && balanced) {
    symbol = symbols[index];

    if (symbol === '(' || symbol === '[' || symbol === '{') {
      stack.push(symbol);
    } else {
      if (stack.isEmpty()) {
        balanced = false;
      } else {
        top = stack.pop();
        if (!match(top, symbol)) {
          balanced = false;
        }
      }
    }
    index++;
  }

  if (balanced && stack.isEmpty()) {
    return true;
  }
  return false;
};
```

### 汉诺塔问题
1、概念: 从左到右有A、B、C三根柱子，其中A柱子上面有从小叠到大的n个圆盘，现要求将A柱子上的圆盘移到C柱子上去，期间只有一个原则：一次只能移到一个盘子且大盘子不能在小盘子上面，求移动的步骤和移动的次数

2、原理: 运用递归，可以将步骤抽象为：
  * (1): 先将A柱的n-1个移动到B柱
  * (2): 再将A柱的最后一个移动到C柱
  * (3): 最后再将B柱的n-1个移动到C柱

3、代码:
```JavaScript
                        // 将from塔的前n个盘子移动到to塔上
var towerOfHanoi = function(n, from, to, helper) {
  if (n > 0) {
    towerOfHanoi(n-1, from, helper, to);
    // 记录步数并输出当前操作
    count++;
    console.log('第' + count + '步: ' + from.peek() + '号盘子: ' + from.name + '---->' + to.name);
    to.push(from.pop());
    towerOfHanoi(n-1, helper, to, from);
  }
};

var source = new Stack(),
    dest = new Stack(),
    helper = new Stack();

source.name = 'A';
dest.name = 'C';
helper.name = 'B';

var num = 3;
for (var i = num; i > 0; i--) {
  source.push(i);
}

var count = 0;
towerOfHanoi(num, source, dest, helper);
```
