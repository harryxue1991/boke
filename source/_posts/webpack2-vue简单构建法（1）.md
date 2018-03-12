---
title: webpack2+vue2简单构建法（1）
date: 2017-10-19 15:04:02
tags:
---

在做前端行业的朋友，估计没有一个人不知道webpack的。因为webpack集开发和打包于一体，操作简单，功能丰富并且强大。作为一名前端工程师，我觉得我们有必要了解并掌握webpack的应用。

由于我们项目主要是应用到vue框架，所以这篇文章是写用怎么用webpack开发vue的

当然现在市面上有很多webpack的脚手架，比方说vue-cli，cooking等，根本不用自己配，直接一个命令，项目就生成了。

然而，存在即是合理的，自己掌握了更多的东西，是不是比以前更强了呢。写这篇文章，当然也是为了见证自己的成长

<!-- more -->

## 开始构建

webpack 有[中文网](https://doc.webpack-china.org/)，里面详细介绍了webpack，如果有不清楚的，可以进去看一下。

目前已知的webpack版本已经是4.0以上了，我只能感慨技术更新快呀。想要不落伍，每天的学习真的是不能停的。

本文就已webpack2为基础，构建一个简单的vue2项目吧。

webpack2p[中文网](http://www.css88.com/doc/webpack2/concepts/entry-points/)

```shell
mkdir demo1
cd demo1
npm init
```

> 注意：这里创建的package.json中，name里不能有webpack，不然后期下载webpack包的时候会报错。

创建完package.json文件后，我们可以在目录下开始搭建文件配置了，先创建以下文件目录，如下图

![avatar](/images/webpack/webpack_demo1.png)

我在github上已写完的demo，请参考：<https://github.com/harryxue1991/my_webpack/tree/master/demo1>

准备完工作，我们就开始下载我们需要的东西

### 安装依赖

```shell
npm install --save vue    
//安装最新版vue
npm install --save-dev webpack@^2.1.0-beta.25 webpack-dev-server@^2.1.0-beta.9
//安装webpack和webpack-dev-server，后者是开发模式下用到的
npm install --save-dev vue-loader vue-template-compiler 
//需要解析.vue后缀的文件，vue开发必备
npm install --save-dev babel-core babel-loader babel-preset-es2015
//es6转码安装
```
### package.json

安装完之后，我们的package.json会将我们刚下载完的统统加在了devDependencies 和 dependencies 对象里
这里我们要配置scripts对象，想要命令启动项目，还得改它：

```json
        "scripts": {
                "dev": "webpack-dev-server --config webpack.config.js",
        },
```
这样我们就可以用 npm run dev 来启动项目了

### webpack.config.js

接下来我们来配置webpack.config.js，如下
```js
// webpack.config.js
const path = require('path');
const webpack = require('webpack');
module.exports = {
        entry: {
                main: path.resolve(__dirname, 'src/main')
                //入口文件
        },
        output: {
                path: path.resolve(__dirname, './dist'),
                publicPath: '/dist/',
                filename: 'build.js'
                //输出文件
        },
        devServer: {
                port: 9999,
                // 端口
                inline: true,
                // 文件更新，页面自动刷新
                historyApiFallback: true
        },
        module: {
                rules: [
                        {
                                test: /\.vue$/,
                                loader: 'vue-loader'
                        },
                        // 装载vue-loader，有了它就能解析.vue文件了
                        {
                                test: /\.js$/,
                                loader: 'babel-loader',
                                /* 排除安装目录的文件 */
                                exclude: /node_modules/
                        }
                         // 装载babel-loader，有了它就能解析es6语法了
                ]
        }
};
```
这里的每个需求都是最基本的项目启动需要的。至于复杂的，我们后面再讲

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
</head>
<body>
        <div id="app"></div>
        <script src="/dist/build.js"></script>
</body>
</html>
```

这个就是我们的html模板，构建的时候，页面js会被打包进来，然后用script引入就可以了。引入的文件名和路径，得看你之前的**webpack.config.js**中的output的配法

### src/App.js

```html
<template>
    <div>
            <div>hello world</div>
    </div>
</template>
<script>
    export default {
        data () {
            
        }
    }
</script>
```
这个文件就是我们要渲染的vue组件

### src/main.js

```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
        el: '#app',
        render: h => h(App)
})
```

引用vue，并实例化一个对象，这个文件就是我们在**webpack.config.js**中的entry的文件

## 使用

配置完文件后，启动看看效果：npm run dev

看到熟悉的hello world，我们的最简单的构建已经完成了。

