---
title: 使用cooking构建项目
date: 2017-04-14 19:33:25
tags:
---

今天我们来介绍一个前端项目架构工具cooking，它是饿了么的一个开源项目，可以快速创建vue或者react项目，非常方便。在使用过程中感觉还蛮好用的，特意来分享给大家。
github地址是：https://github.com/ElemeFE/cooking
中文API地址：http://cookingjs.github.io/zh-cn/index.html

<!-- more -->
### 安装node.js

首先需要安装node环境，可以直接到中文官网http://nodejs.cn/下载安装包。
只是这样安装的 node 是固定版本的，如果需要多版本的 node，可以使用 nvm 安装
http://blog.csdn.net/s8460049/article/details/52396399
安装完成后，可以命令行工具中输入 node -v 和 npm -v，如果能显示出版本号，就说明安装成功。

这里遇到过一个坑，就是安装node8.0以上版本的时候，在后面的构建项目时，node-sass总是安装不上，也许是网速的问题，我回头再试一下，所以我是使用nvm切换成**node 6.9.1**版本的时候，运行没有问题，如下图

![avatar](/images/cooking/node.png)

> 安装cooking

有了node，我们就可以起飞了，全局安装cooking

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

在构建过程中，会有很多选项让你选择，按照自己的需求选择，然后回车即可。或者一路回车也行，这样就构建了简单的vue项目，后期如果想添加功能也是能改的
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
```
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

### 项目打包

```shell
cooking build
```
![avatar](/images/cooking/build.png)

如上图，项目就打包完成了，在根目录下找到打包的文件，该扔哪扔哪。╮(╯▽╰)╭

### 感想

cooking工具感觉还是蛮好用的，在开发一些简单的小项目时，还是可以用用的。因为它简单好用。
当然在一些大型项目里，我觉得还是用webpack之类的工具比较好，因为方便增加或修改自己想要的配置。