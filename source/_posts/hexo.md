---
title: HEXO+github 搭建博客
date: 2019-07-09 11:24:09
tags: hexo
categories: 笔记
comments: true

---

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

<!--more-->

``` bash
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```

### 一、安装

#### 1.安装前提

- Node.js
- Git


#### 2.安装Hexo

` $ npm install -g hexo-cli `

### 二、建站

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

``` bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```


### 三、配置 _config.yml

#### 1.网站


|参数|描述|
| ---- | ---- |
|title|网站标题|
|subtitle|网站副标题|
|description|网站描述|
|author|您的名字|
|language|网站使用的语言|
|timezone|网站时区。Hexo 默认使用您电脑的时区。时区列表。比如说：America/New_York, Japan, 和 UTC 。|
|  |  |

> 其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。


#### 2.网址

|参数|描述|默认值|
| ---- | ---- | ---- |
| url | 网址 |  |
| root	| 网站根目录 |  |
| permalink | 文章的永久链接格式 | :year/:month/:day/:title/ |
| permalink_defaults | 永久链接中各部分的默认值 |  |

> 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。


#### 3.目录

|参数|描述|默认值|
| ---- | ---- | ---- |
| source_dir | 资源文件夹，这个文件夹用来存放内容。 | source |
| public_dir | 公共文件夹，这个文件夹用于存放生成的站点文件。	 | public |
| tag_dir | 标签文件夹 | tags |
| archive_dir | 归档文件夹 | archives |
| category_dir | 分类文件夹 | categories |
| code_dir | Include code 文件夹 | downloads/code |
| i18n_dir | 国际化（i18n）文件夹 | :lang |
| skip_render | 跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。 |  |

### 四、命令

#### 1.init 新建一个网站
如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。

`$ hexo init [folder]`

#### 2.new 新建一篇文章
如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。

`$ hexo new [layout] <title>`

如果标题包含空格的话，请使用引号括起来。

`$ hexo new "post title with whitespace"`

#### 3.generate

生成静态文件 `$ hexo generate`

可简写为： `$ hexo g`

| 选项 | 描述 |
|------|------|
| -d, --deploy | 文件生成后立即部署网站 |
| -w, --watch |	监视文件变动 |

#### 4.publish 发表草稿

`$ hexo publish [layout] <filename>`


#### 5.server 启动服务器

默认情况下，访问网址为： http://localhost:4000/。

`$ hexo server`

| 选项 | 描述 |
|------|------|
| -p, --port | 重设端口 |
| -s, --static |	只使用静态文件 |
| -l, --log |	启动日记记录，使用覆盖记录格式 |

#### 6.deploy 部署网站

`$ hexo deploy`

可简写为：`$ hexo d`

| 选项 | 描述 |
|------|------|
| -g, --generate | 部署之前预先生成静态文件 |

#### 7.render 渲染文件

`$ hexo render <file1> [file2] ...`

| 参数 | 描述 |
|------|------|
| -o, --output | 设置输出路径 |

#### 8.migrate 从其他博客系统迁移内容

`$ hexo migrate <type>`

#### 9.clean 清除缓存文件 (db.json) 和已生成的静态文件 (public)

`$ hexo clean`

在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

#### 10.list 列出网站资料

`$ hexo list <type>`

#### 11.version 显示 Hexo 版本

`$ hexo version`