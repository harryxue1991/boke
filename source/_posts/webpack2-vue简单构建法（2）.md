---
title: webpack2-vue简单构建法（2）
date: 2017-10-24 15:40:47
tags:
---

之前我们构建了简单的webpack2，相信大家可能下载下来试过了。然后一定发现，这个demo1除了能启动预览hello world还能干啥？
好吧，简单的webpack肯定是不能满足我们的需求的，所以我们继续构建能能成运行vue2的webpack吧。

###增加插件html-webpack-plugin

我们为什么要引用它，因为有了它，我们构建的时候，不用在index.html文件里引用添加webapck打包生成的文件了。因为它能自动引入script和link标签。
当然它能配置多个html文件入口。这里我就不细讲了，感兴趣的同学可以网上搜索一下。-_-||

好了，开始下载~~~

```shell

npm install html-webpack-plugin --save-dev

```

下载完，配置文件引入。
![avatar](/images/webpack/webpack_demop2_html.png)
然后配置plugins数组，将引入的插件注册
![avatar](/images/webpack/webpack_demop2_html2.png)
这里的new HtmlWebpackPlugin()构造函数，里面的参数是个对象，下面是我从官网摘的

name | type | default | description
----|----|----|----
title | {String} | `` | The title to use for the generated HTML document
filename | {String} | 'index.html' | The file to write the HTML to. Defaults to index.html. You can specify a subdirectory here too (eg: assets/admin.html)
template | {String} | `` | webpack require path to the template. Please see the docs for details
inject | {Boolean&#124;String} | true | true &#124; 'head' &#124; 'body' &#124; false Inject all assets into the given template or templateContent. When passing true or 'body' all javascript resources will be placed at the bottom of the body element. 'head' will place the scripts in the head element
favicon | {String} | `` | Adds the given favicon path to the output HTML
minify | {Boolean&#124;Object} | true | Pass html-minifier's options as object to minify the output
hash | {Boolean} | false | If true then append a unique webpack compilation hash to all included scripts and CSS files. This is useful for cache busting
cache | {Boolean} | true | Emit the file only if it was changed
showErrors | {Boolean} | true | Errors details will be written into the HTML page
chunks | {?} | ? | Allows you to add only some chunks (e.g only the unit-test chunk)
chunksSortMode | {String&#124;Function} | auto | Allows to control how chunks should be sorted before they are included to the HTML. Allowed values are 'none' &#124; 'auto' &#124; 'dependency' &#124; 'manual' &#124; {Function}
excludeChunks | {String} | `` | Allows you to skip some chunks (e.g don't add the unit-test chunk)
xhtml | {Boolean} | false | If true render the link tags as self-closing (XHTML compliant)

为了更明显地显示这个插件的功能，这里我们改了我们的模板页面，将index.html改为template.ejs，模板页面内容

```html
<!DOCTYPE html>
<html lang="en">
<head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title><%= htmlWebpackPlugin.options.title %></title>
</head>
<body>
        <div id="app"></div>
        <!--assets will be inject bellow-->
</body>
</html>
```

然后，我们需要改一个地方，就是demo1中的output
```js
output: {
        path: path.resolve(__dirname, '/'),
        publicPath: '/',
        filename: '[name].[hash:8].js'
},
```

这里我们将生成的js用后面的8位hash值命名，这样然后publicPath改为根目录，因为我们想在根目录下预览。

### 增加loader，需要预览css和引用图片img

这里，我们在module.rules:[] 这个数组里面，新填了3个loader，加一个插件extract-text-webpack-plugin
这个插件主要将样式单独从js中拎出来，所以我们需要用到它。
因为在开发中我遇到了一个问题，当下载完最新版后，启动报错了。
**chunk.sortModules is not a function**
所以这里我们下载之前的版本就好用，我也是在别人的博客里找到的
``` shell
npm i extract-text-webpack-plugin@2.1.2 --save-dev
```
然后顺便把后面的需要的包全下载了吧。要是遇到下载出错，可能是网络问题，建议用淘宝镜像下载。

``` shell
npm i node-sass css-loader  style-lodader sass-lodader url-lodader file-loader --save-dev
```

这里的node-sass其实蛮坑的，需要你配置python，如果你的电脑没有的话，建议装一个，然后添加电脑的环境变量。
接着把node-sass删掉重装就可以了

```shell
npm uninstall node-sass
npm install node-sass --save-dev
```

好啦，到这里，我们再配置loader，插件的话，在后面的plugins内添加注册

```js

module: {
        rules: [
                {
                        test: /\.css/,
                        use: ExtractTextPlugin.extract({
                                use: 'css-loader'
                        })
                },
                {
                        test:/\.scss$/,
                        use: ExtractTextPlugin.extract({
                                fallback: 'style-loader',
                                use: ['css-loader', 'sass-loader']
                        })
                },
                {
                        test: /\.(png|woff|woff2|eot|ttf|svg|jpg)$/,
                        loader: 'url-loader',
                        options: {
                                limit: 8192
                        }
                }
        ]
}，
plugins: [
        new ExtractTextPlugin('style.[contenthash:8].css')
]
```

写到这里，一个vue基本的开发环境有了。可以参考我的demo2：<https://github.com/harryxue1991/my_webpack/tree/master/demo2>

好吧，我们把webpack增加了点东西，项目启动，完美。但是打包好像还是有点问题。
我们下面再继续探讨