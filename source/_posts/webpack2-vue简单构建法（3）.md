---
title: webpack2-vue2简单构建法（3）
date: 2017-10-25 13:38:19
tags:
---

上一篇文章，我们增加了webpack的一些功能，让我们的项目能正常的运行，但是真正开发的时候，我们还需要打包这些动作。

今天我们再优化下webpack2的配置，将构建和打包分开。

### 配置vendors

我们知道，一个项目里肯定是要引用到一些公共的js的，比方说我们的这个项目，使用了vue。那webpack在打包的时候是不认识这些公共的js的，所以我们自己需要把它们区分开来。
<!-- more -->
webpack内有内置的插件，我们不用再下载了，直接用。还记得插件写哪里吗，就是写在**plugins**里

```js
plugins: [
        new ExtractTextPlugin('style.[contenthash:8].css'),
        //这个是提取css样式的。
        new webpack.optimize.CommonsChunkPlugin({
                name: 'vendors'
        })
]
```

在**plugins**里，新创建了一个构造函数，**new webpack.optimize.CommonsChunkPlugin()**,它的参数里定义了一个name:"vendors"。所以我们在entry对象加上vendors:[]

```js
plugins: [
        entry: {
                main: path.resolve(__dirname, 'src/main'),
                vendors: ['vue']
                //公共代码，插件这类往这里添
        }
]
```

我们再次尝试打开项目，在sources里看到如下图。说明我们成功把js分割开了。
![avatar](/images/webpack/webpack_demo3.png)

### 分割webapck.config.js

首先我们需要将功能分成构建和打包。因为有时候开发模式和最后发布可能会产生不一样的发布路径，若只写在一个文件，很容易出错。

首先，我们将公共部分提取出来，创建一个**webpack.common.js**

```js
const path = require('path');
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

let commonConfig = {
    entry: {
        main: path.resolve(__dirname, 'src/main'),
        vendors: ['vue']
    },
    module: {
        rules: [{
                test: /\.vue$/,
                use: 'vue-loader'
            },
            {
            test: /\.js$/,
            use: [{
                loader: 'babel-loader',
                options: {
                    presets: ['es2015']
                }
            }],
            /* 排除安装目录的文件 */
            exclude: /node_modules/
        },
        {
            test: /\.css/,
            use: ExtractTextPlugin.extract({
                use: 'css-loader'
            })
        },
        {
            test: /\.scss$/,
            use: ExtractTextPlugin.extract({
                fallback: 'style-loader',
                use: ['css-loader', 'sass-loader']
            })
        },
        {
            test: /\.(png|woff|woff2|eot|ttf|svg|jpg)$/,
            loader: 'url-loader',
            options: {
                limit: 8192,
                name: 'images/[hash:8].[name].[ext]'
            }
        }
        ]
    },
    resolve: {
        alias: {
            styles: path.resolve(__dirname, 'src/styles/')
        },
        extensions: ['.js']
    },
    plugins: [
        new ExtractTextPlugin('style.[contenthash:8].css'),
        new HtmlWebpackPlugin({
            template: './template.ejs',
            title: '尖叫蕈',
            inject: 'body'
        }),
        new webpack.optimize.CommonsChunkPlugin({
            name: 'vendors'
        })
    ]
};
module.exports = commonConfig;
```

这里有个**resolve**属性，我们之前没有讲到，它其中的一个功能就是定义变量。

> 以下一个例子

```js
    //配置文件webpack.common.js
    resolve: {
        alias: {
            styles: path.resolve(__dirname, 'src/styles/'),
            image: path.resolve(__dirname, 'src/images/'),
        },
        extensions: ['.js']
    }

    //main.js
    // 引用scr/styles下的scss文件
    import 'styles/main.scss';

    //index.vue
    //引用src/images/下的图片
    <img src="~image/picture.png" alt="">
```

是不是比以前更方便了呢？好吧，我们继续。

公共代码，代表我们构建和打包都用得到的代码。

然后我们再创建一个构建的js：**webpack.dev.js**
添加构建时需要的配置，这里我们需要添加一个插件，用来合并公共部分代码

```shell
npm i webpack-merge --save-dev
```

然后使用require引用刚刚写的公共代码部分，只用merge() 方法合并，这个方法是我们刚刚下载的插件提供的。

> 这里我们补充个东西，就是热更新。

我之前可能没有修改，那就从这里开始改吧。devServer内hot还是要选择true，支持热更新，然后把页面刷新的inline注释掉。
接着plugins内添加webpack本身的HotModuleReplacementPlugin()方法，然后就可以用了

如果嫌shell打印面板内容太多，太乱，可以隐藏，只需要在devServer设置stats:"errors-only"即可。

> 再稍稍补充一个调试

前端开发时必不可少的调试，当然需要加上了。**devtool: '#source-map'**

```js
const merge = require('webpack-merge');
const path = require('path');
const webpack = require('webpack');
const common = require('./webpack.common');

module.exports = merge(common, {
    output: {
        path: path.resolve(__dirname, '/'),
        publicPath: '/',
        filename: '[name].[hash:8].js'
    },
    devtool: '#source-map',
    devServer: {
        port: 9999,
        hot: true,
        // inline: true,
        // 文件更新，页面自动刷新
        host: '0.0.0.0',
        //允许本地ip访问
        historyApiFallback: true,
        stats: "errors-only"
    },
    plugins: [
        new webpack.HotModuleReplacementPlugin()
    ]
})
```

最后我们需要在package.json里修改下script的配置

```json
  "scripts": {
    "dev": "webpack-dev-server --config webpack.dev.js",
  },
```
启动试试看：npm run dev
不出意外的话，完美。

### 分割后的打包配置

我们刚刚创建了构建的js：**webpack.dev.js**，所以我们需要再创建一个打包的js，我们命名：**webpack.prod.js**，配置如下：

```js
const merge = require('webpack-merge');
const path = require('path');
const webpack = require('webpack');
const common = require('./webpack.common');

module.exports = merge(common, {
    output: {
        path: path.resolve(__dirname, 'dist'),
        publicPath: '/',
        filename: '[name].[hash:8].js'
    },
    plugins: [
        new webpack.optimize.UglifyJsPlugin({ compress: {warnings: false})
        // js 压缩
    ]
})
```

这里的ouput:{}中，path属性就是我们最后打包后的路径，所以最好不要放在根目录下，所以这也是我为了和构建环境拆开的原因。

最后在plugins内增加个webpack自带的插件，用来压缩js文件的。
但是在打包的时候我们会要报错的问题。我在网上找了下，说是因为没有编译es5的原因

所以我们在**webpack.common.js**的babel-loader里添加options，如下：

```js
module: {
    rules: [{
        test: /\.js$/,
        use: [{
        loader: 'babel-loader',
        options: {
                presets: ['es2015']
        }
        }],
        /* 排除安装目录的文件 */
        exclude: /node_modules/
    }]
}
```

然后还需要在根目录下添加**.babelrc**文件，内容如下

```json
{
      "presets": ["es2015","stage-0"]
}
```

这里插个话题，就是image打包。我们打包后想把文件放到一个文件夹里，所以我们在url-loader内稍作修改，添加name属性即可：

```js
module: {
    rules: [{
        test: /\.(png|woff|woff2|eot|ttf|svg|jpg)$/,
        loader: 'url-loader',
        options: {
            limit: 8192,
            name: 'images/[hash:8].[name].[ext]'
        }
    }]
}
```

然后在**package.json**添加script属性

```json
  "scripts": {
     "dist": "rm -rf dist/* && webpack --config webpack.prod.js",
  },
```

这样，你在git bash中输入npm run dist就能打包啦。
注：这里如果用shell窗口的话，会出问题，rm不是命令，所以还是要用git bash中输入才行

### 总结

好了，webpack2的基本配置就到这里了，我觉得我们应该也能满足基本的开发需求了。如果想要别的特殊要求，可以继续往里面添加。
当然忘记献上我最后一个demo3的链接了：<https://github.com/harryxue1991/my_webpack/tree/master/demo3>

总的来说，在构建过程中也会踩到坑，不过最终能解决也是蛮开心的一件事，这更加坚定了我不停地学习之路