---
title: live-server —— VSCode如何在localhost运行项目？
date: 2020-05-19 14:29:37
tags: [前端, 插件, 工具, VSCode]
categories: 插件
---

This is a little development server with live reload capability. Use it for hacking your HTML/JavaScript/CSS files, but not for deploying the final site.
这是一个具有实时重新加载功能的小型开发服务器。用它来破解你的HTML/JavaScript/CSS文件，但不是用来部署最终的站点的。

<!--more-->
### 为什么要使用live-server?

- AJAX requests don't work with the file:// protocol due to security restrictions, i.e. you need a server if your site fetches content through JavaScript.
出于安全限制，AJAX请求不能运行在`file://` 协议下，即：如果你的站点通过JavaScript获取内容，那么你需要一个服务器。

- Having the page reload automatically after changes to files can accelerate development.
在文件更改后自动重新加载页面可以加速开发。

- You don't need to install any browser plugins or manually add code snippets to your pages for the reload functionality to work.
这样你就不需要安装任何浏览器插件或者手动地在你的页面中添加代码片段使重新加载功能工作。

-  If you don't want/need the live reload, you should probably use something even simpler, like the following Python-based one-liner: `python -m SimpleHTTPServer`.
如果你不想或者不需要实时重新加载，你可能应该使用更简单的方法，比如下面的基于Python的单行程序：`python -m SimpleHTTPServer`。


### 方法一

#### 1. 安装

```
// 1.npm 安装
$ npm install -g live-server

// 2.git 安装
$ git clone https://github.com/tapio/live-server
$ cd live-server
$ npm install # Local dependencies if you want to hack
$ npm install -g # Install globally
```

#### 2. 运行

```
$ live-server
```

#### 3. 命令行的使用

- `--port=NUMBER`：选择要使用的端口，默认：PORT env var 或 8080
- `--host=ADDRESS`：选择要绑定的主机地址，默认：IP env var 或 0.0.0.0（“任意地址”）
- `--no-browser`：禁止自动启动web浏览器
- `--browser=BROWSER`：指定要使用的浏览器，而不是系统默认值
- `--quiet | -q`：停止日志记录
- `--verbose | -V`：更多的日志记录(记录所有请求，显示所有监听的IPv4接口，等等)
- `--open=PATH`：启动浏览器到PATH而不是服务器根目录
- `--watch=PATH`：逗号分隔的路径字符串，用于专门监视更改(默认:监视所有内容)
- `--ignore=PATH`：要忽略的以逗号分隔的路径字符串(任何匹配兼容的定义)
- `--ignorePattern=RGXP`：要忽略的文件的正则表达式(即.*\.jade)(不符合则忽略)
- `--no-css-inject`：CSS改变时重载页面，而不是注入改变的CSS
- `--middleware=PATH`：导出要添加的中间件功能的.js文件的路径;可以是一个没有路径或扩展名来引用中间件文件夹中的绑定中间件的名称
- `--entry-file=PATH`：服务这个文件(相对于服务器根目录)来代替丢失的文件(对于单个页面应用程序很有用)
- `--mount=ROUTE:PATH`：在已定义的路由下提供路径内容(可能有多个定义)
- `--spa`：translate requests from /abc to /#/abc (handy for Single Page Apps)
- `--wait=MILLISECONDS`：(默认100ms)等待所有更改，然后重新加载
- `--htpasswd=PATH`：启用http-auth，期待位于PATH的htpasswd文件
- `--cors`：为任何源启用CORS(反映请求源，支持带凭据的请求)
- `--https=PATH`：到HTTPS配置模块的路径
- `--https-module=MODULE_NAME`：自定义HTTPS模块(例如spdy)
- `--proxy=ROUTE:URL`：代理所有路由到URL的请求
- `--help | -h`：显示简洁的用法提示并退出
- `--version | -v`：显示版本信息并退出

##### Default options 默认选项：

- 如果文件~/.live-server.json存在，它将被加载并作为命令行上的live-server的默认选项使用。有关选项名称，请参阅“Usage from node”


#### 4.Usage from node

```
var liveServer = require("live-server");
 
var params = {
    port: 8181, // Set the server port. Defaults to 8080.
    host: "0.0.0.0", // Set the address to bind to. Defaults to 0.0.0.0 or process.env.IP.
    root: "/public", // Set root directory that's being served. Defaults to cwd.
    open: false, // When false, it won't load your browser by default.
    ignore: 'scss,my/templates', // comma-separated string for paths to ignore
    file: "index.html", // When set, serve this file (server root relative) for every 404 (useful for single-page applications)
    wait: 1000, // Waits for all changes, before reloading. Defaults to 0 sec.
    mount: [['/components', './node_modules']], // Mount a directory to a route.
    logLevel: 2, // 0 = errors only, 1 = some, 2 = lots
    middleware: [function(req, res, next) { next(); }] // Takes an array of Connect-compatible middleware that are injected into the server middleware stack
};
liveServer.start(params);
```

#### 5.HTTPS

- 为了启用HTTPS支持，您需要创建一个配置模块。模块必须导出一个用于配置HTTPS服务器的对象。这些键与tls.createServer选项中的键相同。

- 例如：

```
var fs = require("fs");
 
module.exports = {
    cert: fs.readFileSync(__dirname + "/server.cert"),
    key: fs.readFileSync(__dirname + "/server.key"),
    passphrase: "12345"
};
```

- 如果使用node API，还可以直接传递一个配置对象，而不是传递到模块的路径。

#### 6.HTTP/2

- 要获得HTTP/2支持，可以通过——HTTPS -module CLI参数(Node.js脚本的httpsModule选项)提供一个定制的HTTPS模块。一定要先安装模块。浏览器不支持HTTP/2未加密模式，因此live-server不支持。看到这个问题，我可以使用页面上的HTTP/2的更多细节。

- For example from CLI(bash)：

```
live-server \
	--https=path/to/https.conf.js \
	--https-module=spdy \
	my-app-folder/
```

#### 7.故障排除

- 修改时未重新加载：
  打开浏览器控制台:顶部应该有一条消息，说明启用了live reload。注意，您需要一个支持WebSockets的浏览器。如果有错误，处理它们。如果它仍然不工作，提出一个问题。

- 错误：watch ENOSPC：
  https://stackoverflow.com/questions/22475849/node-js-what-is-enospc-error-and-how-to-solve/32600959#32600959

- 重新加载工作，但更改丢失或过时
  尝试使用`--wait=MS`选项。其中MS是在发出重载之前等待的时间(以毫秒为单位)。




### 方法二

#### 1. 安装

在vscode中安装Live Server插件。

#### 2. 运行

安装成功后，点击vscode右下角的Go Live标志。


### 参考资料

- Live Server：https://www.npmjs.com/package/live-server