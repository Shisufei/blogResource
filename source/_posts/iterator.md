---
title: Iterator
date: 2020-08-05 16:56:52
tags: [ES6, javaScript]
categories: 笔记
---

**遍历器（Iterator）** 是一种用来处理所有不同的数据结构的机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

<!--more-->

### 一、Iterator

#### 1.Iterator（迭代器）的概念

> **遍历器（Iterator）** 是一种用来处理所有不同的数据结构的机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

##### JavaScript 表示“集合”的数据结构

- Array
- Object
- Map （ES6新增）
- Set （ES6新增）

##### Iterator 的作用

- 为各种数据结构，提供一个统一的、简便的访问接口；
- 使得数据结构的成员能够按某种次序排列；
- ES6 创造了一种新的遍历命令 `for...of` 循环，Iterator 接口主要供 `for...of` 消费。

##### Iterator 的遍历过程

- 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。

- 第一次调用指针对象的 `next` 方法，可以将指针指向数据结构的第一个成员。

- 第二次调用指针对象的 `next` 方法，指针就指向数据结构的第二个成员。

- 不断调用指针对象的 `next` 方法，直到它指向数据结构的结束位置。

> 每一次调用 `next` 方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含 `value` 和 `done` 两个属性的对象。其中：
> - `value` 属性：当前成员的值
> - `done` 属性：一个布尔值，表示遍历是否结束。

#### 2.迭代器协议

> **迭代器协议** 定义了产生一系列值（无论是有限个还是无限个）的标准方式。当值为有限个时，所有的值都被迭代完毕后，则会返回一个默认返回值。

只有实现了一个拥有以下语义的 `next()` 方法，一个对象才能成为迭代器：

- **next()方法**：是一个无参数函数，返回一个拥有 `done` 和 `value` 两个属性的对象。

- **done（boolean）**：如果迭代器可以产生序列中的下一个值，则为 `false` 。如果迭代器已将序列迭代完毕，则为 `true` 。这种情况下，`value` 是可选的，如果它依然存在，即为迭代结束之后的默认返回值。

- **value**：迭代器返回的任何 JavaScript 值。done 为 true 时可省略。

- **注意**：`next()` 方法必须返回一个对象，该对象应当有两个属性： `done` 和 `value`，如果返回了一个非对象值（比如 `false` 或 `undefined`），则会抛出一个 `TypeError` 异常（`"iterator.next() returned a non-object value"`）。

#### 3.可迭代协议

> **可迭代协议** 允许 JavaScript 对象定义或定制它们的迭代行为，例如，在一个 `for..of` 结构中，哪些值可以被遍历到。一些内置类型同时是`内置可迭代对象`或者 `Map` 而其他内置类型则不是（比如 `Object`）。

要成为可迭代对象， 一个对象必须实现 `@@iterator` 方法。

- **[Symbol.iterator]** ：返回一个符合 `迭代器协议` 的对象的无参数函数。

当一个对象需要被迭代的时候（比如被置入一个 `for...of` 循环时），首先，会不带参数调用它的 `@@iterator` 方法，然后使用此方法返回的 **迭代器** 获得要迭代的值。

值得注意的是调用此零个参数函数时，它将作为对可迭代对象的方法进行调用。 因此，在函数内部，`this` 关键字可用于访问可迭代对象的属性，以决定在迭代过程中提供什么。

此函数可以是普通函数，也可以是生成器函数，以便在调用时返回迭代器对象。 在此生成器函数的内部，可以使用 `yield` 提供每个条目。

#### 4.实现一个满足迭代器协议与可迭代协议的对象

```
var letters= {
  cursor: 0,
  next () {
    const actions = ['A', 'B', 'C', 'D']
    if (this.cursor > 7) {
      return {
        done: true
      }
    }
    return {
      done: false,
      value: actions[this.cursor++ % actions.length]
    }
  },
  [Symbol.iterator]: function () {
    return this
  }
}
```
// 实现后可用for…of遍历
```
for (let item of letters) {
  console.log('item:::', item) // done 为 true 时 不执行value中的语句
}
// 输出结果：
// item::: A
// item::: B
// item::: C
// item::: D
// item::: A
// item::: B
// item::: C
// item::: D
```

#### 5.为什么需要2个协议

- 因为不可能知道一个特定的对象是否实现了迭代器协议，然而创造一个同时满足迭代器协议和可迭代协议的对象是很容易的（就像上面的栗子所示）。这样做允许一个迭代器能被不同希望迭代的语法方式所使用。 因此，很少只实现迭代器协议而不实现可迭代协议。


#### 6.实现了迭代器协议与可迭代协议的语法或特性

- `Array`
- `Set`
- `Map`
- `generator`

#### 7.使用了迭代器协议与可迭代协议的语法或特性

- `for...of...`
- `...`
- `Array.from()`

### 二、参考资料

- [《ECMAScript 6 入门》 ](https://es6.ruanyifeng.com/#docs/iterator)
- [《MDN web docs —— 迭代协议》](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols)

