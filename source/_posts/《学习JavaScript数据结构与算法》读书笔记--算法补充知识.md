---
title: 《学习JavaScript数据结构与算法》读书笔记--算法补充知识
date: 2017-10-09 23:22:12
tags: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
keywords: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
categories: 读书笔记
description: 《学习JavaScript数据结构与算法》读书笔记--算法补充知识
toc: true
---

## 递归

**递归**是一种解决问题的方法，它解决问题的各个小部分，直到极倔最初的大问题，通常设计函数自身的调用

递归不会一直无限调用下去，一般都会有递归上限

**尾递归优化**：ES6有尾递归优化，如果函数最后一个操作是调用递归函数，会通过**跳转指令**而不是**子程序调用**来控制

## 动态规划

动态规划(Dynamic Programming, DP)是一种将复杂问题分解成更小问题来解决的优化技术

要注意动态规划和分而治之是不同的方法。分而治之方法是把问题分解成相互独立的子问题，然后组合它们的答案；而动态规划是将问题分解成相互依赖的子问题

解决动态规划问题通常需要三个步骤

  * 定义子问题
  * 实现要反复执行而解决子问题的部分
  * 识别并求解出边界问题

能用动态规划解决的一些著名问题如下：

  * **背包问题**：给出一组项目，各自有值和容量，目标是找出总值最大的项目的集合。这个问题的限制是，总容量必须小于等于“背包”的容量
  * **最长公共子序列**：找出一组序列的最长公共子序列(可由另一序列删除元素但不改变余下元素的顺序而得到)
  * **矩阵链相乘**：给出一系列矩阵，目标是找到这些矩阵相乘的最高效办法(计算次数尽可能少)。相乘操作不会进行，解决方案是找到这些矩阵各自相乘的顺序
  * **硬币找零**：给出面额为d1…dn的一定数量的硬币和要找零的钱数，找出有多少种找零的方法
  * **图的全源最短路径**：对所有顶点对(u, v)，找出从顶点u到顶点v的最短路径

### 最少硬币找零问题

#### 问题

最少硬币找零问题是硬币找零问题的一个变种。硬币找零问题是给出要找零的钱数，以及可用的硬币面额d1…dn及其数量，找出有多少种找零方法。最少硬币找零问题是给出要找零的钱数， 以及可用的硬币面额d1…dn及其数量，找到所需的最少的硬币个数

#### 思路

感觉和斐波那契数列的思想相似。斐波那契数列每个位置的值需要先求其前两项的值。硬币找零也是这个思路。

假如硬币面额是1,3,4，求25的最少硬币找零，这时分三种情况：找一个1元的硬币，然后找24的最少硬币找零；找一个3元的硬币，然后找22的最少硬币找零；找一个4元的硬币，然后找22的最少硬币找零。而24、22、21的最少硬币找零则是重复上面的分析。这样我们就把大问题分解成多个小问题来依次解决。

**在实施的过程中，我们把已经求出来的最少硬币找零数缓存起来来方便后续的使用，同时也提高了效率**

**动态规划实际上就是对可能的情况进行顺序的穷举，然后选取最优解**

#### 代码实现
```javascript
// 动态规划，最少硬币找零问题
function MinCoinCharge (coinsArr) {
  let coins = coinsArr;
  let cache = [];
  // 返回包含应找硬币的数组
  this.makeCharge = (amount) => {
    if (!amount) { return [] }
    if (cache[amount]) { return cache[amount]}
    let min = [], newMin, newAmount;
    for (let i = 0; i < coins.length; i++) {
      const coin = coins[i];
      newAmount = amount - coin;
      if (newAmount >= 0) {
        newMin = this.makeCharge(newAmount);
        if ((newMin.length < min.length -1) || min.length === 0) {
          min = [coin].concat(newMin);
        }
      }
    }
    return cache[amount] = min;
  }
  // 返回最少硬币数和各个硬币的个数
  this.formatCharge = (amount) => {
    const coins_arr = this.makeCharge(amount);
    let result = {};
    coins_arr.forEach(coin => {
      if (Object.keys(result).indexOf(`${coin}`) === -1) {
        result[coin] = 1;
      } else {
        result[coin]++;
      }
    });
    return {
      count: coins_arr.length,
      coins: result,
    };
  }
}
let minCoinCharge = new MinCoinCharge([1,5,10,25]);
console.log(minCoinCharge.makeCharge(36), minCoinCharge.formatCharge(36));
```

## 贪心算法

贪心算法遵循一种近似解决问题的技术，期盼通过每个阶段的局部最优选择(当前最好的解)，从而达到全局的最优(全局最优解)。它不像动态规划那样计算更大的格局。

如下面程序运行，正常最少硬币找零数应该是2，即两张9元硬币。但贪心算法的结果却是5张。很显然并不正确

比起动态规划算法而言，贪心算法更简单、更快。然而，如我们所见，它并不总是得到最优答案。但是综合来看，它相对执行时间来说，输出了一个可以接受的解

### 最少硬币找零(贪心算法版)
```javascript
function MinCoin(coinsArr) {
  let coins = coinsArr;
  this.getCharge = (amount) => {
    let charge = [];
    let total = 0;
    for (let i = coins.length - 1; i >= 0; i--) {
      let coin = coins[i];
      while (total + coin <= amount) {
        charge.push(coin);
        total += coin;
      }
    }
    return charge;
  }
}
const minCoin = new MinCoin([1,5, 9, 10,25]);
console.log(minCoin.getCharge(18));
```

## 大O表示法

我们用大O表示法表示算法的时间复杂度
