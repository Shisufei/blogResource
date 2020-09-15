---
title: MutationObserver
date: 2020-05-14 15:05:23
tags: [前端, Web API 接口]
categories: 笔记
---

`MutationObserver`接口提供了监视对DOM树所做更改的能力。它被设计为旧的`Mutation Events`功能的替代品，该功能是`DOM3 Events`规范的一部分。
<!--more-->

### 构造函数

#### MutationObserver()

- 创建并返回一个新的`MutationObserver`，它会在指定的DOM发生变化时被调用。

### 方法

#### disconnect()

- 阻止`MutationObserver`实例继续接收的通知，直到再次调用其`observe()`方法，该观察者对象包含的回调函数都不会再被调用。

#### observe()

- 配置`MutationObserver`在DOM更改匹配给定选项时，通过其回调函数开始接收通知。

##### example

```
var targetNode = document.getElementById('xxx')

var config = {
  attributes: true,
  childList: true,
  subtree: true
}

var callback = function(mutations){
  mutations.forEach(item => {
    if (item.type === 'childList'){
      console.log('A child node has been added or removed!')
    } else if (item.type === 'attributes'){
      console.log('The ' + item.attributeName + ' attributes was modified!')
    }
  })
}

var observer = new MutationObserver(callback)
observer.observe(targetNode,config)

// 有时也写为如下形式：

new MutationObserver(callback).observe(targetNode,config)

```

#### takeRecords()

- 从`MutationObserver`的通知队列中删除所有待处理的通知，并将它们返回到`MutationRecord`对象的新`Array`中。

### 参考资料

https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver




