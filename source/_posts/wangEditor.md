---
title: 富文本编辑器wangEditor在Vue中的使用
date: 2019-11-08 15:07:47
tags: [前端, Vue, 插件]
categories: 笔记
---

wangEditor是一款基于javascript和css开发的、轻量、简洁、易用、开源免费的Web富文本编辑器。
<!--more-->

## 介绍
- 官网：www.wangEditor.com
- 文档：www.kancloud.cn/wangfupeng/wangeditor3/332599
- 源码：github.com/wangfupeng1988/wangEditor 

## 下载
- 直接下载：https://github.com/wangfupeng1988/wangEditor/releases
- 使用npm下载：npm install wangeditor （注意 wangeditor 全部是小写字母）
- 使用bower下载：bower install wangEditor （前提保证电脑已安装了bower）
- 使用CDN：//unpkg.com/wangeditor/release/wangEditor.min.js

## DEMO
- 下载源码 git clone git@github.com:wangfupeng1988/wangEditor.git
- 安装或者升级最新版本 node（最低v6.x.x）
- 进入目录，安装依赖包 cd wangEditor && npm i
- 安装包完成之后，windows 用户运行npm run win-example，Mac 用户运行npm run example
- 打开浏览器访问localhost:3000/index.html
- 用于 React、vue 或者 angular 可查阅文档中其他章节中的相关介绍

## 在Vue中的使用

### 组件template

```
<template>
  <div>
    <div ref="editor" />
  </div>
</template>
```

### 组件script

```
<script>
import E from 'wangeditor'

export default {
  name: 'Editor',
  props: {
    content: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      editorContent: '',
      editorText: ''
    }
  },
  mounted() {
    var editor = new E(this.$refs.editor)
    editor.customConfig.onchange = (html) => {
      this.editorContent = html
      this.editorText = editor.txt.text()
    }
    // 菜单配置
    editor.customConfig.menus = [ 
      'head', // 标题
      'bold', // 粗体
      'fontSize', // 字号
      'fontName', // 字体
      'italic', // 斜体
      'underline', // 下划线
      'strikeThrough', // 删除线
      'foreColor', // 文字颜色
      'backColor', // 背景颜色
      'link', // 插入链接
      'list', // 列表
      'justify', // 对齐方式
      'quote', // 引用
      'table', // 表格
      'code', // 插入代码
      'undo', // 撤销
      'redo' // 重复
    ]
    // 颜色配置
    editor.customConfig.colors = [
      '#ffffff',
      '#000000',
      '#eeece0',
      '#1c487f',
      '#4d80bf',
      '#c24f4a',
      '#8baa4a',
      '#7b5ba1',
      '#46acc8',
      '#f9963b'
    ]
    editor.create()
    editor.txt.html(this.content) // 回显
  },
  methods: {
    // 获取html
    getContent() {
      return this.editorContent
    },
    // 获取text 可用于判断用户是否输入
    getText() {
      return this.editorText
    }
  }
}
</script>
```

### 使用组件DEMO

template:
```
<template>
  <el-form label-position="left" label-width="100px">
    <el-form-item label="内容">
      <editor ref="editor" :content="form.content" />
    </el-form-item>
  </el-form>
  <el-form-item>
    <el-button type="primary" @click="handleSubmit">提交</el-button>
  </el-form-item>
</template>
```

import:
```
import Editor from '@/components/wangEditor/index'
```

script:
```
components: {
  Editor
},
data() {
  return{
    form:{
      content:''
    }
  }  
},
methods: {
  handleSubmit(){
    this.form.content = this.$refs.editor.getContent()
    // 校验
    if (this.form.content.trim() === '') {
      this.$message({ message: '请填写公告内容！', type: 'warning' })
      return
    }
    const contentText = this.$refs.editor.getText()
    const contentTextArray = contentText.split('&nbsp;')
    const contentTextSet = new Set(contentTextArray)
    contentTextSet.delete('')
    contentTextSet.delete(' ')
    if (contentTextSet.size === 0) {
      this.$message({ message: '请填写公告内容！', type: 'warning' })
      return
    }
  }
}
```

