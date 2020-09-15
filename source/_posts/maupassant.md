---
title: HEXO maupassant 主题使用方法
date: 2019-07-31 11:39:34
tags: hexo
categories: 笔记
comments: true
---


### 一.安装

```
$ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
$ npm install hexo-renderer-pug --save
$ npm install hexo-renderer-sass --save
```

编辑Hexo目录下的 _config.yml，将theme的值改为maupassant。

### 二.配置

#### 1.搜索功能
建议只将其中一种搜索的值配置为true 否则会出现多个搜索框

- google_search  // 使用Google搜索引擎 true/false
- baidu_search  // 使用百度搜索 true/false
- self_search  // 基于jQuery的本地搜索引擎 true/false，需要安装hexo-generator-search插件使用。

```
$ npm install hexo-generator-search --save  // 安装hexo-generator-search插件命令
```

#### 2.捐赠功能

- donate  // 是否启用捐赠按钮 true/false

#### 3.导航菜单

- menu  // 自定义页面及菜单，依照已有格式填写。填写后请在source目录下建立相应名称的文件夹,并包含index.md文件，以正确显示页面。导航菜单中集成了FontAwesome图标字体，可以在这里选择新的图标，并按照相关说明使用。

```
menu:
  - page: about
    directory: about/
    icon: fa-user
```

添加页面：在source目录下建立相应名称的文件夹，然后在文件夹中建立index.md文件，并在index.md的front-matter中设置layout为layout: page。若需要单栏页面，就将layout设置为 layout: single-column。

#### 4.友情链接

- links  // 友情链接，请依照格式填写。

```
links:
  - title: 百度baidu
    url: http://www.baidu.com/
```

#### 5.历史时间线

- timeline  // 网站历史时间线，在页面front-matter中设置layout: timeline可显示。

```
timeline:
  - num: 1
    word: 2014/06/12-Start
  - num: 2
    word: 2014/11/29-XXX
  - num: 3
    word: 2015/02/18-DDD
  - num: 4
    word: More
```

#### 6.canvas动态背景

- canvas_nest  // 是否使用canvas动态背景

```
canvas_nest:
  enable: true ## If you want to use dynamic background please set the value to true, you can also fill the following parameters to customize the dynamic effect, or just leave them blank to keep the default effect.
  color: "100,99,98" ## RGB value of the color, e.g. "100,99,98"
  opacity: "0.7" ## Transparency of lines, e.g. "0.7"
  zIndex: "-1" ## The z-index property of the background, e.g. "-1"
  count: "150" ## Quantity of lines, e.g. "150"
```

Github地址：https://github.com/hustcc/canvas-nest.js

使用方式：
##### 1.npm或cnpm：

`npm install --save canvas-nest.js`

#####  2.yarn: 

`yarn add canvas-nest.js`

#####  3.直接下载文件到本地
#####  4.cdn：

```
<script src="https://cdn.bootcss.com/canvas-nest.js/2.0.4/canvas-nest.js"></script>
```

###### 注：1-3三种方式都需将js文件放在：/themes/source/js目录下，并在需要使用的文章内加入`<script src="/js/canvas-nest.js"></script>`，最好放在文章末尾





<script src="/js/canvas-nest.js"></script>