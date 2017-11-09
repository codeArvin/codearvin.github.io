---
title: 《学习JavaScript数据结构与算法》读书笔记--排序和搜索算法
date: 2017-10-09 22:22:12
tags: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
keywords: [前端, 数据结构, 算法, 《学习JavaScript数据结构与算法》, 读书笔记, JS]
categories: 读书笔记
description: 《学习JavaScript数据结构与算法》读书笔记--排序和搜索算法
toc: true
---

## 排序算法

### 冒泡排序

冒泡排序比较任何两个相邻的项，如果第一个比第二个大，则交换他们。元素向上移动至正确的位置，就好像气泡浮到水面一样，因此得名
**不推荐该算法，算法复杂度为O(n^2)**

```javascript
// 生成随机数组
const getRadomArray = (range, count) => {
  let arr = [];
  const getRange = () => (Math.floor(Math.random() * (range[1] - range[0] + 1) + range[0]));
  for (let i = 0; i < count; i++) {
    arr.push(getRange());
  }
  return arr;
}
// 初始化数组
let array = getRadomArray([1, 100], count);
// 交换数组元素
const swap = (i, j) => {
  let temp = array[i];
  array[i] = array[j];
  array[j] = temp;
}
const len = array.length;
// 冒泡排序
for (let i = len - 1; i >= 0 ; i--) {
  for (let j = 0; j < i; j++) {
    if (array[j] > array[j + 1]) {
      swap(i, j);
    }
  }
}
```

### 选择排序

选择排序算法是一种原址比较排序算法，找出数据结构中的最小值放到第一位，然后找第二小的放到第二位，依此类推
**不推荐该算法，算法复杂度为O(n^2)**

```javascript
// 选择排序
const len = array.length;
let minIndex;
for (let i = 0; i < len - 1; i++) {
  minIndex = i;
  for (let j = i + 1; j < len; j++) {
    if (array[j] < array[minIndex]) {
      minIndex = j;
    }
  }
  if (i !== minIndex) {
    swap(i, minIndex);
  }
}
```

### 插入排序

插入排序假设左侧已经是个有序数组，然后将右侧每一项依次插入左侧数组的正确位置。由于一个元素的数组本身就是排序的，所以初始状态已经有了
**排序小型数组时，插入排序比选择排序和冒泡排序效果好**

```javascript
// 插入排序
const len = array.length;
for (let i = 1; i < len; i++) {
  let j = i;
  const temp = array[i];
  while (j > 0 && array[j - 1] > temp) {
    array[j] = array[j - 1];
    j--;
  }
  array[j] = temp;
}
```

### 归并排序

归并排序是第一个可以被实际使用的排序算法，其算法复杂度为O(nlogn)。
归并排序是一种分治算法，其思想是将原始数组切分成较小的数组，直到每个小数组只有一个位置，接着将小数组归并成较大的数组，直至归并完毕

```javascript
// 归并排序
const merge = (left, right) => {
  let result = [], li = ri = 0;
  const ll = left.length, rl = right.length;
  while (li < ll && ri < rl) {
    if (left[li] < right[ri]) {
      result.push(left[li++]);
    } else {
      result.push(right[ri++]);
    }
  }
  while (li < ll) {
    result.push(left[li++]);
  }
  while (ri < rl) {
    result.push(right[ri++]);
  }
  return result;
}
const mergeSortRec = (array) => {
  const len = array.length
  if (len === 1) {
    return array;
  }
  let mid = Math.floor(len / 2),
      left = array.slice(0, mid),
      right = array.slice(mid, len);
  return merge(mergeSortRec(left), mergeSortRec(right));
}
array = mergeSortRec(array);
```

### 快速排序

快速排序也许是最常用的排序算法了。复杂度为O(nlogn)，且它的性能通常比其他的复杂度为O(nlogn)的排序算法要好
和归并排序一样，快速排序也使用分治的方法，将原始数组分成较小的数组(但它没有像归并排序一样把他们分隔开)。
快速排序先选中一个主元，然后将数组中比主元小的元素移到主元左侧，比主元大的元素移到主元右侧。然后对左右两个数组继续进行相同的操作，直至数组全部排序

```javascript
// 快速排序
const quick = (arr) => {
  if (arr.length <= 1) { return arr }
  const pivotIndex = Math.floor(arr.length / 2);
  const pivot = arr.splice(pivotIndex, 1)[0];
  let left = [];
  let right = [];
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] <= pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }
  return quick(left).concat([pivot], quick(right));
};
array = quick(array);
```

## 搜索算法

### 顺序/线性搜索

顾名思义，依次全部遍历，查找我们需要的元素

```javascript
// 顺序搜索
sequentialSearch = (item) => {
  const len = array.length;
  for (let i = 0; i < len; i++) {
    if (array[i] === item) {
      return i;
    }
  }
  return -1;
}
```

### 二分搜索

二分搜索要求数组是已经排序好的

  * 选择数组的中间值
  * 如果选中值是搜索值，则执行完毕
  * 如果待搜索的值比选中值要小，对左侧数组重复执行上述操作
  * 如果待搜索的值比选中值要大，对右侧数组重复执行上述操作

```javascript
// 二分搜索
binarySearch = (item) => {
  array = quick(array);
  let low = 0, high = array.length - 1, mid, element;
  while (low <= high) {
    mid = Math.floor((low + high) / 2);
    element = array[mid];
    if (element === item) {
      return mid;
    } else if (element < item) {
      low = mid + 1;
    } else {
      high = mid - 1;
    }
  }
  return -1;
}
```
