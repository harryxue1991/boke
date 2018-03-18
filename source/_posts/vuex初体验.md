---
title: vuex初体验
date: 2017-11-21 15:39:07
tags:
---

使用过vue开发的同学，一定听说过vuex，或者已经使用过了。然而我在之前项目其实并没有用到，所以对于vuex这个传说中的神器，一直充满了好奇。

好吧，话不多说，先看[官网文档](https://vuex.vuejs.org/zh-cn/)

本文的demo：<https://github.com/harryxue1991/webpack_vue_demo>

<!-- more -->

### 什么是vuex

这个话题其实官方文档写的很清楚了，相信如果仔细看过vuex文档的同学也一定有所领会。

我的理解是，可以把vuex看成是一个独立于所有页面之外的数据库，所有页面都能访问和修改。这在vue组件化的开发模式下，是非常厉害的一个功能，因为vue同子组件之间的通信，是比较麻烦的一件事。

那我们先来简单理解下vuex的一些功能。

### 目录结构

> 项目布局

```js
.
├── node_modules                                // 包
├── src                                         // 源码目录
//组件
│   ├── components                              // 组件
│   │   └── Header                              // 一个组件
│   │       ├── Header.vue                      // 组件文件
│   │       └── style.scss                      // 一个组件样式
│   ├── images                                  // 公共图片
│   ├── pages                                   // 存放页面的文件夹
│   │   ...
│   │
│   ├── json
│   │   └── person.json                         // 开发阶段的临时数据
│   ├── store                                   // vuex的状态管理
│   │    └── index.js                           // 引用vuex，创建store
│   ├── styles
│   │    ├── common.scss                        // 公共样式文件
│   │    ├── main.scss                          // 全局统一文件
│   │    └── reset.scss                         // 默认清除标签样式1
│   ├── routers.js                              // 路由配置
│   ├── App.vue                                 // 页面模板父组件
│   └── main.js                                 // 程序入口文件，加载各种公共
├── .babelrc                                    // es5需要babel
├── .gitignore                                  // 不需上传文件
├── favicon.ico                                 // 图标
├── package.json                                // package.json
├── README.md                                   // readme文件
├── webpack.common.js                           // webpack公共部分
├── webpack.dev.js                              // webpack构建配置
├── webpack.prod.js                             // webpack打包配置
└── templae.ejs                                 // 入口html文件
.
```

我们先准备一个项目，额，基本都是我之前配的webpack骨架。里面重点是首次引入了store文件夹，里面是我们创建的的store.js
好吧，具体怎么做，我们一步步来。

### 基本配置

我们来看看store.js里最简单的写法

```js
//store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        name: "薛辛超"
    }
})

export default store
```

嗯嗯，的确已经是最简单的写法了。这里的**import Vuex from 'vuex'**是需要我们下载vuex包的，所以我们别忘了

```shell
npm i vuex -S
```

这个是创建一个store的实例，最后export default出来，所以我们要在main.js里把它注册进入vue的实例里

```js
// main.js
import Vue from 'vue'
import store from './store/index.js'

new Vue({
    el: '#app',
    store,
    render: h => h(App)
})
```

像这样就算在vue里面使用到了，当然这里我忽略了vue-router。一个正常的vue单页面开发项目，还是需要vue-router来渲染不同组件的。具体如何配置，可以参考我的demo。

### state的获取

从上面的初步创建，我们创建了一个vuex。vuex一共5个重要属性，分别是state，getter，mutation，action，module。在此我们先了解state的用法。

在之前我们创建的时候，我顺便已经给了一个state对象，里面给了一个name属性

state我这边理解为一个全局的data，所有页面都是可以访问的。所以我们在Header.vue读取一下吧。

```js
// Header.vue
computed: {
    name () {
        return this.$store.state.name
    }
}
```

我们从官方文档上参考，**this.$store.state**就能获取到state这个对象，我们在computed里定义一个变量，一旦值变了，页面内容自然变了。

然后页面上渲染的话，和data里的变量一样用就可以了。

我们渲染出来的内容如下图：
![avatar](/images/vuex/vuex_demo1.png)

拿到值了，开心。

其实到这里，我心里有个疑问。既然它能取代data的功能，那是否可以在整个项目中全部使用vuex来管理数据呢。虽说这样做对于数据来说，只使用一个状态库管理很好，但是vuex是将数据存在内存中的。如果数据量非常大的话，对用户来说是不好的体验，因为吃内存。
所以我觉得应该将公共的数据使用vuex管理，比如说用户信息，再就是购物车之类的，用到的页面特别多。

接着说，官方是推荐更方便的写法，如下：

```js
// Header.vue
import { mapState } from 'vuex';

export default {
    computed: {
        ...mapState({
            name: "name"
            // name: state => state.name
        })
    }
}
```

### mutation

上面我们明白了state的作用，基本就是存数据的，那我们有时需要改值怎么办。在页面直接修改，比方说**this.$store.state.name == '尖叫蕈'**，当然也可以直接修改，但是官方有mutiations方法，可以让我更好的追中每一个状态。

```js
const store = new Vuex.Store({
    state: {
        name: "薛辛超"
    },
    mutations: {
        changeName(state,msg) {
            state.name = msg
        }
    }
})
```

mutations里定义函数，第一个参数能获取state的值，第二个参数是调用函数时可以接受的参数，我们在里面写了个最简单的赋值操作。

当然页面需要调用这个方法，vuex有自己的一个方法，commit()，第一个参数为我们定义的方法名，第二个为传过去的参数。

```js
this.$store.commit('changeName', '尖叫蕈')
```

这里，我直接在**mounted(){}**调用后试试效果，如下图
![avatar](/images/vuex/vuex_demo2.png)

嘻嘻，完美！当然在写法上，vuex方法也有推荐写法

```js
import { mapMutations } from 'vuex';

export default {
    methods: {
        // [
        //     'increment'，
        //     'incrementBy'
        // ]
        ...mapMutations({
            change: 'changeName'，
            // change2: 'changeName2' 可以往后添
        })
    }
}
```

这样，可以传数组，也可以传对象。我个人更喜欢第二种，调用也十分简单，直接this.change(参数)。

```html
<input type="text" v-model="myname" @keydown.enter="change(myname)">
<span v-text="name"></span>
```

额，我这里尝试调用了下方法，效果不错
![avatar](/images/vuex/vuex_demo3.png)

但是这里只能处理同步方法，官方文档说，是因为在 mutation 中混合异步调用会导致你的程序很难调试。例如，当你能调用了两个包含异步回调的 mutation 来改变状态，你怎么知道什么时候回调和哪个先回调呢？这就是为什么我们要区分这两个概念。

### getter

getter是用于过滤数据用的。