---
title: 使用cooking构建项目
date: 2017-04-14 19:33:25
tags:
---

今天我们来介绍一个前端项目架构工具cooking，它是饿了么的一个开源项目，可以快速创建vue或者react项目，非常方便。在使用过程中感觉还蛮好用的，特意来分享给大家。
github地址是：<https://github.com/ElemeFE/cooking>
中文API地址：<http://cookingjs.github.io/zh-cn/index.html>

<!-- more -->

### 安装node.js

首先需要安装node环境，可以直接到中文官网<http://nodejs.cn/>下载安装包。
只是这样安装的 node 是固定版本的，如果需要多版本的 node，可以使用 nvm 安装
<http://blog.csdn.net/s8460049/article/details/52396399>
安装完成后，可以命令行工具中输入 node -v 和 npm -v，如果能显示出版本号，就说明安装成功。

![avatar](/images/cooking/node.png)

**官方要求：首先确保是在 NPM 3+, Node 4+, Python 2.7+ 环境下运行**，所以在安装前，确保npm，node和python已经下载完并且版本都是在要求的最低之上

> 安装cooking

第一步，全局安装cooking，没有什么问题

```shell
npm i cooking-cli -g
```

安装完成，查看cooking版本是否能显示，注意，这里的V是大写的哦~

![avatar](/images/cooking/cooking_version.png)

### 生成项目

完成安装了cooking，我们的任务已经完成一大半了，是不是感觉太简单了。o(*￣︶￣*)o
接下去我们构建并生成项目，只需要一个命令

```shell
cooking create my-project vue
```

如果想构建react项目，只要吧vue改成react即可

```shell
cooking create my-project react
```

这里我遇到过一个**问题**，如下图：

![avatar](/images/cooking/cooking_create.png)
因为我的电脑系统重装了，然后node这些都是重新装的，为了方便管理，node是使用nvm管理版本，使用的node版本是v8.9.3版本。
按照github上issue解答：首先确保npm3+，然后把~/.cooking目录删掉，重新安装并执行 cooking就能解决。

但是……并没有卵用。/(ㄒoㄒ)/~~

确实，在我们开发过程中遇到这种问题真的头疼，可能因为一个小问题导致失败的，但这个小问题就是难找……

然后，我确定我的环境是官方要求的：**NPM 3+, Node 4+, Python 2.7+ 环境**，在这里我给大家承认一个错误，就是我当时系统重装后，没有安装python，我觉得可能是这个原因导致的。于是我安装完之后，再安装cooking-cli，还是不行。
在这里我们需要将npm 或 cnpm 清理下缓存

```shell
npm cache clean
cnpm cache clean
```

清理缓存后继续安装，还是同样的问题。

既然上帝给你关闭一扇门，那你可以打开一扇窗。我在github上看到issue上有同样遇到问题的人，但是开发者并没有关闭issue。纳尼？难道是最新版本就是有问题的？

于是我用nvm 切换到node7.10.1版本（反正我版本多，随便浪）。继续下载低版本的cooking-cli

```shell
npm i cooking-cli@1.5.0 -g
```

安装完，命令**cooking create my-project vue**，神奇的事情发生了，构建没问题了。/(ㄒoㄒ)/~~，感谢上帝。
然后我再切换回node8.9.3版本，竟然构建同样没问题了。
好吧，既然问题解决了，我也不纠结到底哪里有问题了。

接着讲,在构建过程中，会有很多选项让你选择，按照自己的需求选择，然后回车即可。或者一路回车也行，这样就构建了简单的vue项目，后期如果想添加功能也是能改的
然后慢慢地等吧……

如果在构建过程中网速不行卡住了，或者出错了，那应该是包没下载下来。好解决，简单暴力打开项目

```shell
cd my-project
```

然后把**node-modules**删除，再自己手动下载一遍，推荐使用cnpm，或者淘宝镜像

```shell
npm install --registry=https://registry.npm.taobao.org
```

这样操作后，启动项目（下面会讲）我们会发现一个问题，报错了
![avatar](/images/cooking/cooking_error.png)
遇到问题先不要慌，看问题怎么描述的：意思大致是如果我们使用了vue-lodader, 需要更新**vue-template-compiler**
找到问题了，那更新呗

```shell
npm install vue-template-compiler --Save
```

下载完，启动cooking watch，完美。

### 启动项目

启动项目命令

```shell
cooking watch
```

![avatar](/images/cooking/start.png)

如上图，项目启动成功，如果出现端口被占用的情况，我们可以在根目录下找到**cooking.conf.js**里修改端口，如下图。

![avatar](/images/cooking/port.png)

好吧，浏览器打开<http://127.0.0.1:8080>，现在可以开开心心地撸代码了。
当然**cooking.conf.js**内的配置参数，可以参考[官方文档](http://cookingjs.github.io/zh-cn/configuration.html)，里面有更详细的解释。

在第一次启动的时候，在**cooking.conf.js**内的extends的数组内，如果有sass选项的话，如下图
![avatar](/images/cooking/cooking_sass.png)
在项目启动的时候会安装自动插件sass，然后会经常安装失败，这样项目就无法启动了。
作为老司机的我遇到这样的问题也很头痛。
其实它自动安装的sass是**node-sass**插件，所以安装不上可能是网速问题，我们可以先手动下载安装，如果能安装成功，那再运行命令就问题不大。

```shell
npm uninstall node-sass
npm install node-sass
```

node-sass还是蛮奇葩的，需要先卸载再安装。如果还是失败，试着用cnpm或者加上淘宝镜像。
再不行，可能你电脑没有安装python。
若是安装了python还是不行，那你确定下你的电脑环境有没有添加python。
怎么添加python环境变量……自行百度吧，百度上有太多教程了，一搜一大堆。。。

### 项目打包

```shell
cooking build
```

![avatar](/images/cooking/build.png)

如上图，项目就打包完成了，在根目录下找到打包的文件，该扔哪扔哪。╮(╯▽╰)╭

### 感想

cooking工具感觉还是蛮好用的，在开发一些简单的小项目时，还是可以用用的。因为它简单好用。
当然在一些大型项目里，我觉得还是用webpack之类的工具比较好，因为方便增加或修改自己想要的配置。