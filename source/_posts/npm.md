---
title: npm
date: 2020-08-05 16:57:03
tags: [前端,NodeJS]
categories: 笔记
---

**npm** 是 `Node.js` 标准的软件包管理器。它起初是作为下载和管理 Node.js 包依赖的方式，但其现在也已成为前端 JavaScript 中使用的工具。

<!--more-->

## npm 

- **node package manager**（node包管理工具）

- **npm** 是 `Node.js` 标准的软件包管理器。

- 它起初是作为下载和管理 Node.js 包依赖的方式，但其现在也已成为前端 JavaScript 中使用的工具。

- **yarn** 是 `npm` 的一个替代选择

### 下载

- **npm** 可以管理项目依赖的下载

#### 一、安装所有依赖

如果项目具有 package.json 文件，则运行：

```
npm install
```

它会在 node_modules 文件夹（如果尚不存在则会创建）中安装项目所需的所有东西。

#### 二、安装单个软件包

```
npm install <package-name>
```

- **--save** ：安装并添加条目到 package.json 文件的 dependencies对象中

- **--save-dev** ：安装并添加条目到 package.json 文件的 devDependencies对象中

#### 三、更新软件包

##### 1.更新全部：

- npm 会检查所有软件包是否有满足版本限制的更新版本。

```
npm update
```

##### 2.更新单个

```
npm update <package-name>
```
