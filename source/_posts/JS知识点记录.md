---
title: JS知识点记录
date: 2017-04-29 21:26:23
tags: [JS]
keywords: [JS]
categories: 学习记录
description: 记录平时遇到的JS的各种知识点
toc: true
---
## 进制转换
`parseInt(string, radix)`: 将string代表的数转换成10进制数，返回值类型为number
  * `string`: 要被解析的值，如果不是字符串，则将其转换成字符串，字符串开头的空白会被忽略
  * `radix`: 2-36之间的整数值，表明string代表的数的基数。

`Number.toString(radix)`: 返回数的指定基数的字符串表示，默认radix为10

我的理解:
  * parseInt是把字符串转换成数值表示，他的radix用来指定string对应的数的基数，转换后返回的数基数是10。也就是**返回一个字符串的10进制数值表示**。例如`parseInt('1100100',2)`返回`100`
  * Number.toString是把数值转换成字符表示，radix用来指定转换后的字符对应的数的基数。也就是**返回一个数的radix基数的字符表示**。例如`var num = 100; num.toString(2)`返回`'1100100'`

## 原型
![](JS知识点记录/prototype.jpg)
结合图来理解，一点一点的看。[参考资料](http://www.cnblogs.com/smoothLily/p/4745856.html)

1、在JS里，万物皆对象(typeof null === 'object'是个bug，其他像string、number类型也能用方法，应该是在定义后，编译器会自动用String、Number函数将其包装起来。例如`var x = 'hello'; x.__proto__ === String.prototype //true`)。**对象都拥有属性`__proto__`**，称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型。如图中f1、f2、o1、o2是由构造函数生成的，他们的`__proto__`属性指向其构造函数的prototype属性。原型对象也是对象，他的`__proto__`属性指向Object.prototype。

2、函数也是对象，但它除了拥有`__proto__`属性外还拥有自己的特有属性prototype，称为原型属性。这个属性指向一个对象(突然发现`typeof Function.prototype === true`，这就很纠结了，这个问题等待解决中。**update at 2017/09/22**)，称为原型对象，**原型对象有一个属性constructor指向原构造函数**。函数也是对象，Foo的构造函数是Function，所以Foo.`__proto__` === Function.prototype。还有如Object.`__proto__` === Function.prototype、比较特殊的有Function.`__proto__` = Function.prototype

3、Object.`__proto__` === null
