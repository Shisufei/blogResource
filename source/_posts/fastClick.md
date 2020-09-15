---
title: FastClick的使用
date: 2019-08-20 11:58:37
tags: 插件
categories: 插件
---

移动设备上的浏览器默认会在用户点击屏幕大约延迟300毫秒后才会触发点击事件，这是为了检查用户是否在做双击。为了能够立即响应用户的点击事件，才有了FastClick。

<!--more-->

> mobile browsers will wait approximately 300ms from the time that you tap the button to fire the click event. The reason for this is that the browser is waiting to see if you are actually performing a double tap.

### FastClick的使用

#### 一、安装

安装fastclick可以使用npm，Component和Bower。另外也提供了Ruby版的gem fastclick-rails以及.NET提供了NuGet package。例：

1.在页面引入fastclick js文件：
```
<script type='application/javascript' src='/path/to/fastclick.js'></script>
```
2.使用npm安装
```
npm install fastclick
```

#### 二、初始化FastClick实例

初始化FastClick实例建议在页面的DOM文档加载完成后。

1. 纯Javascript版

```
if ('addEventListener' in document) {
    document.addEventListener('DOMContentLoaded', function() {
        FastClick.attach(document.body);
    }, false);
}
```

2. jQuery版

```
$(function() {
    FastClick.attach(document.body);
});
```

3. 类似Common JS的模块系统方式

```
var attachFastClick = require('fastclick');
attachFastClick(document.body);
```

调用require('fastclick')会返回FastClick.attach函数。

#### 3.使用needsclick过滤特定的元素

如果页面上有一些特定的元素不需要使用fastclick来立刻触发点击事件，可以在元素的class上添加needsclick:

```
<a class="needsclick">Ignored by FastClick</a>
```

### 兼容性

该库已作为FT Web App的一部分进行部署，并在以下移动浏览器上进行了测试：

- iOS 3及更高版本的Mobile Safari
- 适用于iOS 5及更高版本的Chrome
- Android上的Chrome（ICS）
- Opera Mobile 11.5及更高版本
- 自Android 2以来的Android浏览器
- PlayBook OS 1及以上版本

### 不需要使用FastClick的情况

以下这几种情况是不需要使用FastClick：

```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

- FastClick不会对PC浏览器添加监听事件
- Android版Chrome 32+浏览器，如果设置viewport meta的值为width=device-width，这种情况下浏览器会马上出发点击事件，不会延迟300毫秒。
- 所有版本的Android Chrome浏览器，如果设置viewport meta的值有user-scalable=no，浏览器也是会马上出发点击事件。但请注意，user-scalable=no还会禁用缩放缩放，这可能是一种可访问性问题。
- IE11+浏览器设置了css的属性touch-action: manipulation，它会在某些标签（a，button等）禁止双击事件，IE10的为-ms-touch-action: manipulation


