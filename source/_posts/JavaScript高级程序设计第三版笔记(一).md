---
title: JavaScript高级程序设计读书笔记（一）
date: 2018-01-14 20:08:00
tags: [JavaScript高级程序设计]
keywords: [JS]
categories: 读书笔记
description: JavaScript高级程序设计（第三版）读书笔记
toc: true
---

# 一、JavaScript 简介
1. 虽然JavaScript和ECMAScript通常都被人们用来表达相同的含义，但JavaScript的含义却比ECMA-262中规定的要多得多。一个完整的JavaScript应该由下面三个不同的部分组成
    * **核心（ECMAScript）**: 由ECMA-262定义，提供核心语言功能
    * **文档对象模型（DOM）**: 提供访问和操作网页内容的方法和接口。DOM1、DOM2、DOM3，功能目标依次更加宽泛
    * **浏览器对象模型（BOM）**: 提供与浏览器交互的方法和接口

# 二、在HTML中使用JavaScript
1. 所有`script`元素都会按照他们在页面中出现的先后顺序依次被解析。在不使用`defer`和`async`属性的情况下，只有在解析完前面`script`元素中的代码之后，才会开始解析后面`script`元素中的代码
2. `defer`告诉浏览器立即下载，但脚本会被延迟到整个页面都解析完毕后再运行
3. `async`，一旦脚本可用，会异步执行，不会阻塞页面的渲染
4. `defer`和`async`都只适用于外部脚本

# 三、基本概念
1. 区分大小写
2. 标示符：
  * 第一个字符必须是一个字母、下划线(_)或一个美元符号($)
  * 其他字母可以是字母、下划线、美元符号或数字
3. `var`声明的变量~~是当前作用域的变量~~会被自动添加到最接近的环境中，不使用`var`声明的是全局变量
4. 5种简单数据类型(基本数据类型): `Undefined`, `Null`, `Boolean`, `Number`, `String`；1种复杂数据类型：`Object`
5. `typeof`是一个操作符而不是一个函数
6. `3.215e7` `3e-17`
7. `0.1 + 0.2 = 0.30000000000000004`
8. `NaN`不等于任何值，包括它本身，我们用`isNaN()`来判断某个变量是否"**不是数值**"
9. 基于对象调用`isNaN()`时，先会调用对象的`valueOf()`方法，然后确定该方法返回的值是否可以转换为数值。如果不能，则基于这个返回值再调用`toString()`方法，再测试返回值
10. 数值转换：`Number()`, `parseInt()`, `parseFloat()`
11. 调用数值的`toString()`方法时，可以传递一个参数：输出数值的基数
    ```javascript
    var num = 10;
    num.toString(); // "10"
    num.toString(2); // "1010"
    num.toString(8); // "12"
    num.toString(10); // "10"
    num.toString(16); // "a"
    ```
12. 位操作符
  * `~`: 按位非(NOT)，返回数值的反码
  * `&`: 按位与(AND)
  * `|`: 按位或(OR)
  * `^`: 按位异或(XOR)
  * `<<`: 左移
  * `>>`: 有符号右移，即移动的时候保留符号位，符号位不参与移动
  * `>>>`: 无符号右移，即符号位参与移动
13. 逻辑与与逻辑或都属于短路操作，即如果第一个操作数能够决定结果，**那么就不会对第二个操作数进行求值**
14. `break`: 立即退出循环，强制继续执行循环后面的语句；`continue`: 立即退出循环，但退出循环后从循环的顶部继续执行
15. ECMAScript中的参数在内部是用一个数组来表示的

# 四、变量、作用域和内存问题
1. ECAMScript中所有函数的参数都是**按值传递的**
  > 经过查阅思考后，暂时得出结论：**js中所有变量都是按值传递的，按值传递我理解为`a = b`，b把他的值复制一份传给a，这两个值互相独立，互不干扰**。
  >对值类型，变量的值直接存在栈内存中，读取的时候直接去该变量对应的栈内存位置读取；对引用类型，变量对应的栈内存位置存的是实际值的堆内存地址，实际值存在堆内存中，读取的时候先去该变量的栈内存位置找到堆内存地址，然后根据这个地址去堆内存中找到实际的值，也就是引用类型的储存和读取都多了一步。所以在变量的复制时，传递的都是变量对应栈内存中储存的东西，值类型就直接是变量的值，复制完互不干扰；引用类型复制的就是内存地址，复制完也互不干扰，但读取的时候相同内存地址的值仍然是相同的。而参数的传递也就是从实参到形参的复制
  > 参考资料：[前端基础进阶：详细图解 JavaScript 内存空间](https://juejin.im/entry/589c29a9b123db16a3c18adf) |  [JavaScript深入之参数按值传递 ](https://github.com/mqyqingfeng/Blog/issues/10)
2. **执行环境**(execution context)定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的**变量对象**(variable object)，环境定义的所有变量和函数都会保存在这个对象中。虽然我们无法访问这个对象，但解析器在处理数据时会在后台使用它
3. 每个函数都有自己的**执行环境**，当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行后，栈将其环境弹出，把控制权返还给之前的执行环境
4. 当代码在一个环境中执行时，会创建对象的一个**作用域链**(scope chain)，作用域链用来保证对执行环境有权访问的所有变量和函数的**有序访问**。作用域链的前端始终都是当前执行代码所在环境的**变量对象**。如果这个环境是函数，则将其**活动对象**(activation object)作为变量对象。活动对象最开始只包含一个变量，即`arguments`对象(在全局环境中是不存在的)。作用域链中的下一个变量对象来自包含(外部)环境，而在下一个变量对象则来自下一个包含环境。这样一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象
5. **标示符**的解析是从作用域的前端开始，逐级的向后回溯，直到找到标示符为止
6. `try-catch`中的`catch`块和`with`语句会在作用域的前端临时增加一个变量对象，该变量对象会在代码执行后被移除
  * `catch`：创建一个新的变量对象放在前端，其中包含的是被抛出的错误对象的声明
  * `with`：会将指定的对象添加到前端
7. ES5之前没有块级作用域，ES6开始有了块级作用域(需使用`let`和`const`)
8. JS具有自动垃圾收集机制。原理很简单，找出那些不再继续使用的变量，然后释放其占用的内存。为此，垃圾收集器会按照固定的时间间隔(或代码执行中预定的收集时间)，周期性的执行这一操作
9. 用于标记无用变量的策略会因实现而异，但具体到浏览器中的实现，通常有两个策略：
  * **标记清除**：最常用的垃圾收集方式，当变量进入环境时标记为"进入环境"，离开环境时标记为"离开环境"
  * **引用计数**：跟踪记录每个值被引用的次数，当变成0的时候就可以回收。但如果用到**循环引用**的话就会出现问题
10. 确定一个值是哪种基本类型可以使用`typeof`操作符；而确定一个值是哪种引用类型可以使用`instanceof`操作符
11. 变量的执行环境有助于确定应该何时释放内存
12. 解除变量的引用不仅有助于消除循环引用现象，而且对垃圾收集也有好处。为了确保有效的回收内存，应该及时解除不再使用的全局变量、全局对象属性以及循环引用变量的引用

# 五、引用类型
1. 数组的`length`属性很有特点——**他不是只读的**。因此，通过设置这个属性，可以从数组的末尾移除项或向数组中添加新项
2. 检测数组：正常我们使用`value instanceof Array`来判断一个变量是否是数组，但这假定是在单一的全局执行环境下，如果网页包含多个框架，那实际上就存在两个版本以上不同的全局执行环境，从而存在两个以上不同版本的`Array`构造函数。如果你从一个框架向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生创建的数组分别具有不同的构造函数。为了解决这个问题，ECMAScript 5新增了一个方法`Array.isArray()`，用来检测一个变量是否是数组，无论它是在哪个全局执行环境中创建的
3. 数组的`toLocaleString()`、`toString()`、`valueOf()`方法分别会调用数组中每一项的`toLocaleString()`、`toString()`、`valueOf()`方法；`push()`和`unshift()`方法可以接受多个参数；`sort()`默认升序排列数组项，并调用每一项的`toString()`，即使是数值类型，可以穿入一个比较函数`(a,b) => {return }`，如果a应该在b前面返回负数，如果两个参数相等返回0(**???**)，如果a应该在b后面返回正数
4. 数组的`concat`方法连接数组实际上是连接数组的浅拷贝，**如果你连接的数组里面包引用类型**，如果你改变原引用，连接后的引用也会改变
    ```javascript
    var a = [{a: 1}, 2],
        b = [[3, 4], 5];
    c = a.concat(b); // [{a: 1}, 2, [3, 4], 5]
    a[0].b = 2; // [{a: 1, b: 2}, 2, [3, 4], 5]
    b[0][0] = 233; // [{a: 1, b: 2}, 2, [233, 4], 5]
    b = [3,4]; // 注意这里b失去了对原引用的绑定，所以c没有变化
    ```
5. 数组`reduce`和`reduceRight`如果不传入第二个参数，迭代从数组第二项开始；如果传入第二个参数，迭代从数组第一项开始
6. 由于传递给RegExp函数的参数是字符串(不能传递表达式字面量)，所以某些情况下需要对字符进行双重转义，如：字面量`/\[bc\]at/`，等价的字符串`"/\\[bc\\]at/"`；而且使用字面量和使用RegExp构造函数创建的正则表达式不一样，在ECMAScript 3中，正则表达式字面量始终共享一个RegExp实例，而使用构造函数创建的每一个实例都是一个新实例(**在ECMAScript 5中做了修改，使用字面量必须像直接调用构造函数一样，每次都创建新的RegExp实例**)
7. `exc()`方法在不设置全局标志的情况下，在同一个字符串上多次调用`exec()`始终会返回第一个匹配项的信息；在设置全局标志的情况下，每次调用`exec()`都会在字符串中继续查找新匹配项。同时它的`lastIndex`(代表开始搜索下一个匹配项的字符位置)会发生变化
8. ECMAScript中没有函数重载
9. 实际上，解析器在向执行环境中加载数据时，对函数声明和函数表达式并非一视同仁。解析器会率先读取函数声明，并使其在执行任何代码之前可用(可以访问)；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解析执行
10. 函数的名字仅仅是一个包含指针的变量而已
11. `arguments`是一个类数组对象，包含着传入函数中的所有参数，它还有一个名叫`callee`的属性，该属性是一个指针，指向拥有这`arguments`对象的函数，通过`arguments.callee`可以解除该函数与函数名的耦合
12. `caller`是函数对象的一个属性，保存着调用当前函数的函数的引用
13. 函数的`length`属性表示函数希望接收的命名参数的个数
14. `prototype`属性不可枚举，使用`for-in`无法发现
15. `apply`的第一个参数用来设置函数体内部`this`对象的值；第二个参数是一个数组，用来表示传入函数的参数
16. `call`的第一个参数和`apply`的一样，与之不同的是接下来的参数用来表示传入函数的参数，即传给函数的参数必须逐个列举出来
17. 传递参数并非是`apply`和`call`真正的用武之地：他们真正强大的地方是能够扩充函数赖以运行的作用域，即可以指定函数的`this`
18. `bind`用来绑定函数的`this`
19. 三种基本包装类型：`Boolean`、`Number`、`String`。当第二行访问s1时，访问过程处于一种访问模式，也就是要从内存中读取这个字符串的值。而在读取模式中访问字符串时，后台都会自动完成下列处理：
  1. 创建`String`类型的一个实例
  2. 在实例上调用指定的方法
  3. 销毁这个实例
    ```javascript
    var s1 = 'some text';
    var s2 = s1.substring(2);
    ```
  **也是经过这种处理，js中的基本类型就变得和对象一样拥有各种方法和属性了**
20. 引用类型和基本包装类型的主要区别就是**对象的生存期**，使用`new`操作符创建的引用类型的实例，在执行流离开当前作用域前都一直保存在内存中。而自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立即销毁。这意味着我们不能在运行时为基本类型值添加属性和方法
21.
```javascript
var value = '25';
var number = Number(value); // 转型函数
typeof number; // 'number'

var obj = new Number('value'); // 构造函数
typeof obj; // 'object'
```
22. `Number`类型的几个方法
  * `toFixed`: 按照指定的小数位返回数值的字符串表示
  * `toExponential`: 返回1以指数表示法(e表示法)表示的数值的字符串形式，接收一个参数指定输出结果中的小数位数
  * `toPrecision`: 返回合适的数值的字符串表示，可能固定大小格式，可能指数格式，接受一个参数指定表示数值的所有数字的位数(不包括指数部分)
23. `Global`对象是ECMAScript中最特别的一个对象了，因为不管你从什么角度看，这个对象都是不存在的。ECMAScript中的`Global`对象在某种意义上是作为一个终极的"兜底儿对象"来定义的。换句话说，不属于任何其他对象的方法和属性，最终都是作为它的属性和方法。事实上，没有全局对象和全局函数；所有在全局作用域中定义的属性和函数，都是`Global`对象的属性。诸如`isNaN()`，`isFinite()`，`parseInt()`以及`parseFloat()`，实际上都是`Global`对象的方法。除此之外，`Global`对象还包含其他一些方法：
    * URL编码方法：`encodeURI()`主要用于整个URL(例如：http://www.wrox.com/illegal value.htm)，而`encodeURIComponent()`主要用于对URL中的某一段(例如前面URL中的illegal value.htm)进行编码。他们的主要区在于，`encodeURI()`不会对本身属于URL的特殊字符进行编码，例如冒号、正斜杠、问号和井字号；而`encodeURIComponent()`则对它发现的任何非标准字符进行编码。例如：
    ```javascript
    var uri = 'http://www.wrox.com/illegal value.htm#start';

    encodeURI(uri); // 'http://www.wrox.com/illegal%20value.htm#start' 除了空格以外的其他字符都原封不动
    encodeURIComponent(uri); // 'http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start' 替换所有非字母数字字符
    ```
    对应的有`decodeURI()`和`decodeURIComponent()`分别用来解码`encodeURI()`和`encodeURIComponent()`编码的字符串
    * `eval()`

# 六、面向对象的程序设计
1. 对于只有内部才用的特性，规范把它们放在了两个方括号中，例如`[[Enumerable]]`
2. ECMAScript中有两种属性：数据属性和访问器属性
  * **数据属性**：数据属性包含一个数据值的位置。在这个位置可以读取和写入值。数据属性有4个描述其行为的特性：
    1. `[[Configurable]]`：表示能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问其属性。直接在对象上定义的属性该特性默认值为`true`
    2. `[[Enumerable]]`：表示能否枚举，能否通过`for-in`循环返回属性。直接在对象上定义的属性该特性默认值为`true`
    3. `[[Writable]]`：表示能否修改属性的值。直接在对象上定义的属性该特性默认值为`true`
    4. `[[Value]]`：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。默认值为`undefined`
    要修改这几个特性，必须使用`Object.defineProperty()`，如：
    ```javascript
    var person = {};
    Object.definedProperty(person, 'name', {
      configurable: false,
      value: 'hello'
    });
    person.name; // 'hello'
    delete person.name;
    person.name; // 'hello'
    ```
    注意，一旦把属性定义为不可配置的，就再也不能把它变回可配置的了
  * **访问器属性**：访问器属性不包含数据值；他们包含一对`getter`和`setter`函数(这两个函数都不是必须的)，在读取访问器属性的时候，会调用`getter`函数，这个函数负责返回有效的值；在写入访问器属性的时候，会调用`setter`函数并传入新值，这个函数负责决定如何处理数据。访问器属性有如下4个特性：
    1. `[[Configurable]]`：与数据属性的对应特性相同
    2. `[[Enumerable]]`：与数据属性的对应特性相同
    3. `[[Get]]`：在读取属性时调用的函数。默认值为`undefined`
    4. `[[Set]]`：在写入属性时调用的函数。默认值为`undefined`
    ```javascript
    var book = {
      _year: 2004,
      edition: 1
    };
    Object.defineProperty(book, 'year', {
      get: function () {
        return this._year;
      },
      set: function (newValue) {
        if (newValue > 2004) {
          this._year = newValue;
          this.edition += newValue - 2004;
        }
      }
    });
    book.year = 2005;
    book.edition; // 2
    ```
    访问器属性不能直接定义，必须使用`Object.defineProperty()`来定义
    **修改`year`属性也会同时修改`_year`和`edition`的值，这是使用访问器属性的常用方式，即设置一个属性的值会导致其他属性发生变化**
3. 对于`Object.defineProperty()`方法定义的属性，没有指定的描述符默认值为`false`或`undefined`
4. `Object.defineProperties(obj. props)`方法**通过描述符一次定义多个属性**
5. `Object.getOwnPropertyDescriptor(obj, prop)`方法取得给定属性的描述符；`Object.getOwnPropertyDescriptors(obj, prop)`方法返回所有属性的描述符
6. 创建对象：
  1. **工厂模式**：工厂模式虽然解决了创建多个相似对象的问题，但没有解决对象识别的问题(即怎样知道一个对象的类型)
  ```javascript
  function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    return o;
  }
  ```
  2. **构造函数模式**：构造函数需要使用new操作符。以这种方式调用构造函数实际上会经历以下4个步骤：
    * 创建一个新对象
    * 将构造函数的作用域赋给新对象(因此this就指向了这个新对象)
    * 执行构造函数中的代码
    * 返回新对象
  ```javascript
  function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
  }
  var person1 = new Person('nicholas', 29, 'Doctor');
  ```
  构造函数与其他函数的唯一区别就在于调用他们的方式不用。不存在定义构造函数的特殊语法。任何函数，只要通过new操作符来调用，那它就可以作为构造函数；而任何函数，如果不通过new操作符来调用，那它跟普通函数也没有任何区别。
  ```javascript
  var person = new Person('nicholas', 29, 'doctor'); // 作为构造函数来调用

  Person('nicholas', 29, 'doctor'); // 添加到window
  window.name; // nicholas

  var o = new Object();
  Person.call(o, 'nicholas', 29, 'doctor'); // 在另一个对象的作用域中调用
  o.name;
  ```
  3. **原型模式**：我们创建的每一个函数都有一个`prototype`(原型)属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法
  ```javascript
  function Person() {}
  Person.prototype.name = 'nicholas';
  Person.prototype.age = 29;
  Person.prototype.job = 'doctor';

  var person1 = new Person();
  ```
  无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个`prototype`属性，这个属性指向函数的原型对象。在默认情况下，所有原型对象都会自动获得一个`constructor`(构造函数)属性，这个属性包含一个指向`prototype`属性所在函数的指针。如`Person.prototype.constructor === Person; // true`
  创建了自定义的构造函数之后，其原型对象默认只会取得`constructor`属性，至于其他方法都是从`Object`继承而来。当调用一个构造函数创建一个新实例后，该实例的内部将包含一个指针(内部属性)，指向构造函数的**原型对象**，没有标准的方式访问`[[Prototype]]`，但浏览器中可以使用`__proto__`来进行访问
  访问对象的属性时，会沿着其原型链向上进行搜索，直至找到或没找到
  **重写原型对象切断了现有原型与之前任何已经存在的对象实例之间的联系；他们引用的仍然是最初的原型**
  `isPrototypeOf()`: `Person.prototype.isPrototypeOf(person1) // true`
  `Object.getPrototypeOf()`: `Object.getPrototypeOf(person1) == Person.prototype // true`
  `hasOwnProperty()`: 可以检测一个属性是存在于实例中，还是存在于原型中
  `Object.getOwnPropertyNames()`: 用来得到所有实例属性，无论是否可枚举
  4. **组合使用构造函数模式和原型模式**：
  ```javascript
  function Person(name) {
    this.name = name;
  }
  Person.prototype = {
    constructor: Person,
    sayName: function() {
      return this.name;
    }
  }
  var person1 = new Person('hello');
  ```
  5. **动态原型模式**：*还是不太明白和上一个模式的区别。。。*
  ```javascript
  function Person(name) {
    this.name = name;

    if (typeof this.sayName != 'function') {
      Person.prototype.sayName = function () {
        return this.name;
      }
    }
  }
  var person1 = new Person('hello');
  ```
  6. **寄生构造函数模式**：*不太明白它和工厂模式的区别，为什么会出现这种模式*
  ```javascript
  function Person(name) {
    var o = new Object();
    o.name = name;
    return o;
  }
  var person1 = new Person('hello');
  ```
  除了使用new操作符并把使用的包装函数叫做构造函数以外，这个模式跟工厂模式没有任何区别
  构造函数在没有返回值的情况下，默认会返回新对象实例。而通过在·构造函数的末尾添加`return`，可以重写调用构造函数时返回的值
  这个模式可以在特殊的情况下用来为对象创建构造函数。假设我们想创建一个具有额外方法的特殊数组。由于不能直接修改`Array`构造函数，因此可以使用这个模式：
  ```javascript
  function SpecialArray() {
    var values = new Array();
    values.push.apply(values, arguments);
    values.toPipedString = function () {
      return this.join('|');
    }
    return values;
  }
  ```
  7. **稳妥构造函数模式**: 所谓**稳妥对象**，指的是没有公共属性，而且其方法也不引用`this`的对象。稳妥对象最适合在一些安全的环境中(这些环境中会禁止使用`this`和`new`)，或者在防止数据被其他应用程序改动时使用。稳妥构造函数遵循着与寄生构造函数类似的模式，但有两点不同：一是新创建对象的实例方法不引用`this`；二是不使用`new`操作符调用构造函数
  ```javascript
  function Person(name) {
    var o = new Obeject();

    o.sayName = function () {
      return name;
    }
    return o;
  }
  ```
7. **继承**：许多OO语言都支持两种继承方式：**接口继承**和**实现继承**。接口继承只继承方法签名，而实现继承则继承实际的方法。ECMAScript中无法实现接口继承，它只支持实现继承，主要依靠原型链来实现
  1. **原型链**：实例的原型对象的原型对象的···，构成了原型链
  2. **借用构造函数**：在子类型构造函数内部调用超类型构造函数
  ```javascript
  function SuperType(name) {
    this.name = name;
  }
  function SubType() {
    SuperType.call(this, 'hello');
    this.age = 29;
  }
  ```
  3. **组合继承**：有时候也叫伪经典继承，指的是将原型链和借用构造函数的技术组合到一起，从而发挥两者之长的一种继承模式。其背后的思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又能够保证每个实例都有自己的属性
  ```javascript
  function SuperType(name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
  }
  SuperType.prototype.sayName = function () {
    return this.name;
  };

  function SubType(name, age) {
    SuperType.call(this, name); // 继承属性，但我看其实就是为共有操作进行函数封装
    this.age = age;
  }
  // 继承方法
  SubType.prototype = new SuperType();
  SubType.prototype.sayAge = function () {
    return this.age;
  }
  ```
  4. **原型式继承**：借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型
  ```javascript
  function object(o) {
    function F() {}
    F.prototype = o;
    return new F();
  }
  ```
  本质上讲，`object()`对传入的对象进行了一次浅复制
  如果只想让一个对象与另一个对象保持类似的情况下，原型式继承是完全可以胜任的
  5. **寄生式继承**：与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，对后再像真的是它完成了所有工作一样返回对象(*PS:话说寄生是不是就是在强大对象身上添加点功能，然后把这强大了点的对象当作自己的功劳*)
  ```javascript
  function createAnother(original) {
    var clone = object(original);
    clone.sayHi = function () {
      return 'hi';
    }
    return clone;
  }
  ```
  6. **寄生组合式继承**：组合继承无论在什么情况下，都会调用两次超类型构造函数。为了解决这个问题，我们提出了寄生组合式继承：即通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。其背后的思路是，不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非是超类型的一个副本而已。本质上，就是使用寄生式继承模式来继承超类型的原型，然后再将结果指定给子类型的原型(*PS:区别也就是一种使用new操作符来链接原型，一种使用赋值操作来链接原型，第二种方法的好处是超类型中不会存在多余的属性*)
  ```javascript
  function inheritPrototype(subType, superType) {
    var prototype = object(superType.prototype);
    prototype.constructor = subType;
    subType.prototype = prototype;
  }
  ```
8. PS：感觉继承的这些方法都是在向一个方向不断优化的，这个方向是：通过方法创建的实例，独特的属性值自己拥有，共享的属性方法放在原型链上，实现尽可能减少不必要的开支

# 函数表达式
