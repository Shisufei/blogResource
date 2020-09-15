---
title: nvm避坑指南
date: 2020-07-07 11:45:23
tags: [前端,NodeJS]
categories: 笔记
---

使用场景：比如我们手上同时在做好几个项目，这些项目的需求都不太一样，导致了这些个项目需要依赖的nodejs版本也不同，这种情况下，我们就可以通过nvm来切换nodejs的版本，而不需要频繁地下载/卸载不同版本的nodejs来满足当前项目的要求

<!--more-->

## nvm

- node version manager（node版本管理工具）

- 通过将多个node 版本安装在指定路径，然后通过 nvm 命令切换时，就会切换我们环境变量中 node 命令指定的实际执行的软件路径。

- 使用场景：比如我们手上同时在做好几个项目，这些项目的需求都不太一样，导致了这些个项目需要依赖的nodejs版本也不同，这种情况下，我们就可以通过nvm来切换nodejs的版本，而不需要频繁地下载/卸载不同版本的nodejs来满足当前项目的要求

### windows系统下的nvm 安装

#### 1. 下载

- 链接：[https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases)

**可下载以下版本：**

- nvm-noinstall.zip：绿色免安装版，但使用时需要进行配置。

- nvm-setup.zip：安装版，推荐使用

#### 2. 安装（nvm-setup)

1. 双击解压后的文件`nvm-setup.exe`

 ![image.png](https://upload-images.jianshu.io/upload_images/3143447-18dde214f03c838c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 选择nvm安装路径

![image.png](https://upload-images.jianshu.io/upload_images/3143447-0dfa65a973b0b292.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**遇坑警告！！！！！**

> 这里我一开始选择的路径为 `C:\Program Files\nvm`
在安装完成后，执行 `nvm use 14.5.0` 的时候，出现了：
`exit code 1:‘D:\Program’ #%$#^%$^%&%&@#`
的问题（后面这些符号表示当时报错时候的乱码……）
查阅了一些前人的经验后得知，问题的原因是`Program Files` 这个文件名中含有空格@_@
所以，各位在选择文件夹的时候，需要注意，文件夹名**不要出现** `中文` 和 `空格`。

3. 选择nodeks安装路径

![image.png](https://upload-images.jianshu.io/upload_images/3143447-9a0e3eb6f1dc00c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 确认安装

![image.png](https://upload-images.jianshu.io/upload_images/3143447-720b87d16f85a331.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.检查是否安装成功

打开CMD，输入`nvm`，安装成功则会如下图所示，它会显示出当前nvm版本以及nvm的命令：

![image.png](https://upload-images.jianshu.io/upload_images/3143447-b159a5689f1001da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 3. 使用nvm

1. nvm list 命令 - 显示版本列表

```
nvm list // 显示已安装的版本（同 nvm list installed）
nvm list installed // 显示已安装的版本
nvm list available // 显示所有可以下载的版本
```

2. nvm install 命令 - 安装指定版本nodejs

```
nvm install 14.5.0 // 安装14.5.0版本node
nvm install latest // 安装最新版本node
```

3. nvm use 命令 - 使用指定版本node

```
nvm use 14.5.0 // 使用14.5.0版本node
```

4. nvm uninstall 命令 - 卸载指定版本 node

```
nvm uninstall 14.5.0 // 卸载14.5.0版本node
```

**遇坑警告！！！！！**

> 在运行`nvm install` 的时候，有可能会出现无权限安装的问题，如果遇到此问题，请 `以管理员身份运行` cmd。

#### 4. 其他命令

1. `nvm arch` ：显示node是运行在32位还是64位系统上的

2. `nvm on` ：开启nodejs版本管理

3. `nvm off` ：关闭nodejs版本管理

4. `nvm proxy [url]` ：设置下载代理。不加可选参数url，显示当前代理。将url设置为none则移除代理。

5. `nvm node_mirror [url]` ：设置node镜像。默认是https://nodejs.org/dist/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

6. `nvm npm_mirror [url]` ：设置npm镜像。https://github.com/npm/cli/archive/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

7. `nvm root [path]` ：设置存储不同版本node的目录。如果未设置，默认使用当前目录。

8. `nvm version` ：显示nvm版本。version可简化为v。







