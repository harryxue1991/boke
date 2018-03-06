---
title: pdf.js初体验
date: 2017-06-05 14:50:12
tags:
---

在写这篇文章前，先感谢我的同事飞哥。在使用pdf.js前，公司有个需求要求做一个能预览并翻页pdf的页面。在此之前，只用过PDFObject插件，但它的效果并不是很好，最后效果就是一个iframe引用pdf文件，每个浏览器的默认样式都不同，而且还没法调用它浏览器自带的方法（可能是我没找到），总之也弄得我焦头烂额。
这时候飞哥将pdf.js推荐给我，并且指导我完成开发，让我获益匪浅。

### pdf.js下载

pdf.js 能将pdf渲染成canvas，所以是基于html5平台展示的，现在主流浏览器基本没啥问题。
pdf.js 官方演示demo预览: http://mozilla.github.io/pdf.js/web/viewer.html
pdf.js 官方下载：http://mozilla.github.io/pdf.js/

<!-- more -->

下载完成后，解压，看到如下图。
![avatar](/images/pdfjs/pdfjs_1.png)

### pdf.js 使用

拿到build 和 web文件夹我们很懵逼(⊙o⊙)…，但是不要慌。打开build里发现有两个文件：**pdf.js** 和 **pdf.worker.js**。这两个文件是最重要的两个文件，一个负责API解析，一个负责核心解析。
然后我们再打开web文件夹，里面有个viewer.html文件，于是，我们开心地点开，额，点开后发现pdf并不能预览。
因为这个页面需要请求文件，需要在服务器的环境下才能运行。所以我们将build文件夹移到apache服务器的www根目录下（当然随便什么web服务器都行）。
双击打开后，pdf能预览了。

拿到demo后怎样解读呢。其实这个确实挺麻烦的，看[官方文档](http://mozilla.github.io/pdf.js/examples/)呗

> 我们先写个简单的渲染。

首先，如果只是渲染pdf页面的话，其实只要两个js就可以了。就是**pdf.js** 和 **pdf.worker.js**。按照官方例子。以下是我的demo1，可以在我的github直接下载<https://github.com/harryxue1991/mypdf/tree/master/demo1>

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
        <div id="viewer" style="width: 1000px"></div>
</body>

</html>
<script src="./pdf.js"></script>
<script type="text/javascript">
        function showPdf() {
                var url = 'abc.pdf';    
                 //引用pdf文件
                PDFJS.workerSrc = './pdf.worker.js';  
                 //引用pdf.worker.js文件
                PDFJS.getDocument(url).then(function (pdf) {
                        var viewer = document.getElementById("viewer");
                        //需要渲染的的位置div，这里用了原生方法获取id
                        for (var i = 1; i < pdf.numPages; i++) {  
                        //for循环pdf页, 这里的numPages为pdf总页码，因为pdf没有第0页
                        // 所以我们得从第一页开始渲染
                                pdf.getPage(i).then(function (page) {
                                        // 如果只是渲染一页的话，这里的getPage()方法传变量
                                        //然后点击传入所要页面数，页面就会渲染哪一页，非常酷炫
                                        var desiredWidth = viewer.clientWidth;
                                        //这里设置我们需要渲染出pdf的大小，单位是px，但这里我们不需要写单位。
                                        var viewport = page.getViewport(1);
                                        //这里传的参数1，主要我想获取我引入pdf的尺寸大小（主要取宽度）
                                        // 得到的viewport对象，里面就有实际pdf的尺寸
                                        var scale = desiredWidth / viewport.width;
                                        // 将我需要的尺寸，除以实际尺寸，scale得到比例
                                        var scaledViewport = page.getViewport(scale);
                                        //将得到的比例再传入这个方法中，告诉pdf.js，我要渲染这么大的
                                        var canvas = document.createElement('canvas');
                                        // 创建一个canvas
                                        var context = canvas.getContext('2d');
                                        // canvas基本方法
                                        canvas.width = viewer.clientWidth;
                                        // 设置canvas的宽
                                        canvas.height = scaledViewport.height * viewer.clientWidth /
                                                scaledViewport.width;
                                        // 设置canvas的长，按照比例计算就可以了
                                        var renderContext = {
                                                canvasContext: context,
                                                viewport: scaledViewport
                                        };
                                        // 创建一个对象，设置属性和值，这些值我们上面都得到了
                                        page.render(renderContext);
                                        // 然后渲染，这个时候是渲染出canvas标签，我们当然要插入到我们的div里啦，所以还差一步
                                        viewer.appendChild(canvas);
                                        // 添加进入我们的div里面
                                });
                        }
                })
        }
        showPdf();
</script>
```

上面的代码我做了很详细的注释，细心的同学会发现，咦，这样渲染出的pdf，不能选中文字啊。我想说：是的，这样就只是一个pdf渲染成canvas画布的方法，想要做到和demo一样可以抓取文字，那还需要多做一些。但是我们公司需求只是用来渲染的话，应该是足够了的。

> pdf.js高级，文字渲染版

对于上面的简单版，文字渲染版就高级多了。当然并不是我们需要多敲代码，只是我们得多引入js和css。
大家还记得上面有个web文件夹吧，它里面存放了一个demo，直接是文字渲染版的，当初飞哥就是从demo里剥离代码的，厉害！
我现在重新看一下**viewer.js**中代码，需要将pdf的demo的功能一点点剥掉，真的是很麻烦的一件事。我不知道飞哥是怎么弄的，捂脸笑哭~~。
好了，不废话了，上代码。这是demo2，github地址：<https://github.com/harryxue1991/mypdf/tree/master/demo2>，里面的pdf1.html，打开就有效果

```html
<!DOCTYPE html>
<html lang="en">

<head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <link rel="stylesheet" href="./viewer.css">
        <title>Document</title>
</head>

<body>
        <div id="viewer" style="width: 1000px"></div>
</body>

</html>
<script src="./pdf.js"></script>
<script src="./viewer.js"></script>
<script type="text/javascript">
        var _pdf;
        var url = './abc.pdf';
        PDFJS.workerSrc = './pdf.worker.js';
        PDFJS.getDocument(url).then(function (pdf) {
                _pdf = pdf;
                console.log(pdf.numPages); //pdf总页数
        }).then(function () {
                showPdfPage(1);
                //所传参数，就是我们需要渲染的页数。注意，参数范围是1-总页数，传参前需要做限制。
        });
        function showPdfPage(pageNum) {
                var container = document.getElementById("viewer");
                _pdf.getPage(pageNum).then(function (page) {
                        var scale = 1;
                        var viewport = page.getViewport(scale);
                        // scale还是倍数，先使用1倍获取原文件pdf宽度
                        scale = container.clientWidth / viewport.width;
                        // 需要页面宽度除以原pdf宽度，获得倍数
                        var pageView = new PDFPageView({
                                container: container,
                                id: page.pageNumber,
                                scale: scale,
                                defaultViewport: viewport,
                                // renderingQueue: this.renderingQueue,
                                textLayerFactory: new DefaultTextLayerFactory(),
                                annotationLayerFactory: new DefaultAnnotationLayerFactory(),
                                //enhanceTextSelection: this.enhanceTextSelection,
                                //renderInteractiveForms: this.renderInteractiveForms,
                        });
                        //这个构造函数就是我们引入的viewer.js里定义的。只要将我们获得的参数按照对象形式传入，就可以使用了。
                        pageView.setPdfPage(page);
                        pageView.draw();
                });
        }
</script>
```

以上是渲染单页的，也是我项目中最常用的，如果想要渲染多页。那就和我的demo1一样，写个for循环就可以了。以下示例：
github地址：<https://github.com/harryxue1991/mypdf/tree/master/demo2> ，pdf2.thml就是

```html
<!DOCTYPE html>
<html lang="en">

<head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <link rel="stylesheet" href="./viewer.css">
        <title>Document</title>
</head>

<body>
        <div id="viewer" style="width: 1000px"></div>
</body>

</html>
<script src="./pdf.js"></script>
<script src="./viewer.js"></script>
<script type="text/javascript">
        var url = './abc.pdf';
        var container = document.getElementById("viewer");
        PDFJS.workerSrc = './pdf.worker.js';
        PDFJS.getDocument(url).then(function (pdf) {
                for(var i = 1; i < pdf.numPages; i++) {
                        pdf.getPage(i).then(function (page) {
                                var scale = 1;
                                var viewport = page.getViewport(scale);
                                scale = container.clientWidth / viewport.width;
                                var pageView = new PDFPageView({
                                container: container,
                                id: page.pageNumber,
                                scale: scale,
                                defaultViewport: viewport,
                                textLayerFactory: new DefaultTextLayerFactory(),
                                annotationLayerFactory: new DefaultAnnotationLayerFactory(),
                                });
                                pageView.setPdfPage(page);
                                pageView.draw();
                        });
                }
        })        
</script>
```

### 结尾

好了，相信看到这里的小朋友，对pdf.js一定也有所了解了，如果只是想简单地渲染，就只需用到我的这几个demo就可以了，如果还需要更多的操作功能，那还是得找文档，或者自己慢慢地去挖掘。
学习是一件快乐的事，写到这里，我由衷的感到幸福。(#^.^#)
