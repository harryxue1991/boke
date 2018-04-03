---
title: nuxt.js初体验
date: 2017-12-21 12:00:27
tags:
---

前后端分离开发是比较常见的开发模式，但是SEO优化是一个问题。在我们公司开发项目的时候，因为seo优化，最后选择的是前后端不分离开发。这样的开发模式下，代码总会显得很乱，往往前端开发完的代码，后端还需要看一遍逻辑，对于开发的效率来说，也很低下。

所以，既想seo优化，又想前后端分离，需要前端多做一些功课。

如果用nodejs做前端项目开发，利用目前前端比较流行的express或者koa开发，也是一个比较好的方案。但是开发中配置vue-ssr也是比较麻烦的一件事，具体我没有配过，感兴趣同学自己可以试下。

当然进入我们的主题，nuxt.js是一个基于vue.js的服务端渲染用用框架，因为它使用起来非常简单，也容易上手，所以我们接下去来体验一下吧。

<!-- more -->

[官方API](https://zh.nuxtjs.org/)

### 安装

官方提供了我们一个starter模板 <https://github.com/nuxt-community/starter-template>，当然我们也可以自己创建模板，利用vue-cli安装使用：（vue-cli安装:  npm i vue-cli -g）

```shell
vue init nuxt-community/starter-template <project-name>
cd <project-name>
npm install or yarn
npm run dev
```

按照如上这样步骤，只要不出错就能启动项目（出错的话，一般会是包下载问题，只要重新再下就行了，推荐用yarn），默认是3000端口，打开 http://localhost:3000 试试效果。

![avatar](/images/nuxt/nuxt_1.png)

我们看到能正常访问了。

> 修改端口

我们默认端口是3000端口，如果想修改端口，打开**package.json**，添加一个**config**设置就行了

```json
  "config": {
    "nuxt": {
      "host": "0.0.0.0",
      "port": "3333"
    }
  }
```

改完然后重启，项目地址端口就改为3333了。

### 项目的重要配置目录

> pages

打开我们的模板页面，看到**pages**文件夹，这也是最重要的一个文件夹，里面是放我们所有的页面。
文件夹下所创建的文件名和文件夹名，会自动被解析成路由。

如：pages/user/index.vue，我们访问  http://localhost:3000/user 就能访问到。

> store

store文件夹内是放vuex文件的，我们之前已经学过vuex的用法，那这里写法上完全没啥问题，而且也不用单独去下载vuex，因为nuxt已经默认帮我们下载好了。

在store文件夹下创建index.js，就算激活了vuex功能。

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = () => new Vuex.Store({

  state: {
    counter: 0
  },
  mutations: {
    increment (state) {
      state.counter++
    }
  }
})

export default store
```

写法上和我们正常用vuex开发简直是一模一样，但是写完还是要重启项目才能看到效果。

> layouts

一个模板文件夹，相当于我们在使用webpack中使用的APP.vue差不多的功能，当然实际是有点不一样的。如下：

```js
<template>
  <nuxt/>
</template>
```

**<nuxt/>**是我们在之前pages中的.vue文件渲染出来的位置，所以如果我们在外面套一个div，然后再设置一些自己需要的内容，不就是一个模板了吗。

在layouts中，有个默认的错误页面error.vue 文件，那是当访问出错后，自动跳转到这一页的。

```js
<template>
  <div class="container">
    <h1 v-if="error.statusCode === 404">页面不存在</h1>
    <h1 v-else>应用发生错误异常</h1>
    <nuxt-link to="/">首 页</nuxt-link>
  </div>
</template>

<script>
export default {
  props: ['error'],
  layout: 'blog' // 你可以为错误页面指定自定义的布局
}
</script>
```

当然，我们的个性化布局也是需要的。如果我们创建了layouts/blog.vue文件，只需要在pages内的.vue文件里设置布局就可以了

```js
<script>
export default {
  layout: 'blog'
}
</script>
```

> 其他文件夹

assets是用于组织未编译的静态资源如 LESS、SASS 或 JavaScript。components 是存放组件的。static 是存放静态文件的，plugins是存放插件的地方，我们后面会细说。

### 视图

服务器渲染还有一个比较重要的就是TDK了，我们在开发的时候，只需要在js中添加head属性就可以了

```js
  head() {
    return {
      title: this.title,
      titleTemplate: '%s - Nuxt.js',
      meta: [
        { hid: 'description', name: 'description', content: '这是descroiption，自定义的' },
        { hid: 'Keywords', name:'Keywords',content: '宅客学院,IT职业教育,大数据培训,Hadoop培训,Spark培训'}
      ]
    }
  }
```

我们在nuxt.config.js内可以设置一个模板（其实不设的话，nuxt默认是给我们一个模板的）,然后我们在页面内设置的head会覆盖nuxt.config.js的配置，这样的话，也是比较灵活的。

> 异步数据


### 压缩打包

当我们项目构建完毕后，nuxt提供了打包和生产模式的命令

package.json配置：

```json
{
  "name": "my-app",
  "dependencies": {
    "nuxt": "latest"
  },
  "scripts": {
    "dev": "nuxt",
    "build": "nuxt build",
    "start": "nuxt start"
  }
}
```

发型部署:

```shell
nuxt build
nuxt start
```