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

### getter

getter是用于对数据进行过滤，查找用的。它可以理解为，我们不用vuex前的computed属性，只是我们用了vuex，data属性已经用到了computed属性，那么它就多给我一个getters 属性用来给我们过滤。

我们在store里添加getters属性

```js
// store/index.js
    state: {
        like: [
            {"id": "1",'Fruit': "apple"},
            {"id": "2",'Fruit': "banner"},
            {"id": "3",'Fruit': "origin"}
        ]
    },
    doneLike: (state) => (id) => {
        return state.like.find(todo => todo.id == id)
    }
```

我们在这里给它返回一个函数，传入一个参数，当参数和like数组id一样的时候，返回当前id的那一项。

好的。我们在页面里再调用getters方法。其实最简单的是**this.$store.getter.doneLike(1)**，这样就能获取到一个对象字面量。

vuex同样的getter也给我了一个方法

```html
<!-- index.vue -->
    <div class="name2">小葱喜欢吃</div>
    <div class="fruit" v-text="doneLike(3).Fruit"></div>

<script>
    import { mapGetters } from 'vuex'
    export default {
        computed: {
            ...mapGetters([
                'doneLike',
            ])
        },
    }
</script>
```

这样就能显示效果了，如下图
![avatar](/images/vuex/vuex_demo4.png)

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

### action

上次说到mutation是处理同步代码逻辑的。那要是遇到异步操作怎么办。vuex有个action属性，它不变更状态，但是可以提交mutation，而且是专门用来处理异步操作的。

我们先写**store/index.js**文件，首先引入axios，因为要异步请求数据

```js
//store/index.js
import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'
Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        person: {}
    },
    mutations: {
        changeperson(state,msg) {
            state.person = msg
        }
    },
    actions: {
        getMsg({ commit }) {
            axios({
                method: 'get',
                url: 'src/json/person.json'
            }).then((res) =>{
                commit('changeperson',res.data)
            })
        }
    }
})

export default store
```

这里这个action是利用axios请求本地的一个json文件，然后请求成功后，调用mutations的方法，给state中person赋值。

接着，我们在页面中应用。

```html
<!-- home.vue -->
<el-button type="primary" @click="addMyGril">按我触发异步请求</el-button>
<div v-if="!_.isEmpty(person)">
    <div v-text="person.name"></div>
    <div v-text="person.age"></div>
    <div v-text="person.girlFriend"></div>
</div>

<script>
    import { mapState, mapActions} from 'vuex';
    export default {
        computed: {
            ...mapState({
                person: 'person'
            })
        },
        methods: {
            addMyGril() {
                this.$store.dispatch('getMsg')
            }
        }
    }
</script>
```

我页面的逻辑是，点击触发actions里定义的getMsg，将获取到的person渲染出来。点击试了下效果，如下：
![avatar](/images/vuex/vuex_demo5.png)

很好，获取到数据了，说明我们的异步很成功。这里说个题外话，在此刻，我写到小葱的时候，其实'小葱'不是我们吃的小葱，而是我的相亲对象。我们虽然没见过面，不过聊了快一个月了，她给我的感觉，很开朗的一个女孩，只是最近她或许有点忙，一直没怎么聊天，可能她不中意我吧。我觉得不管以后怎样结果，至少此刻，我对她是有好感的，也希望她以后能幸福。哎~一种老实人的独白。

接着讲，当然vuex也给我们提供了更简单的页面触发actions方法的写法

```js
// store/index.js
import { mapState, mapActions} from 'vuex';

 methods: {
    ...mapActions([
        'getMsg'
    ]),
    addMyGril() {
        this.getMsg()
        // this.$store.dispatch('getMsg')
    }
}
```

这样写法上，就更省力了。

### modules

最后一个modules，它的出现可以看成是为了缓解一个store的臃肿，将数据更加精细化分类。

store里写法，在外面新建一个**second.js**，里面写了所有的state,mutations, actions等，如下：

```js
// store/second.js
const moduleA = {
    namespaced: true,
    state: {
        count: 0
    },
    mutations: {
        increment(state) {
            // 这里的 `state` 对象是模块的局部状态
            state.count++
        }
    },
    getters: {
        doubleCount(state) {
            return state.count * 2
        }
    }
}
export default moduleA;
```

这里的**namespaced: true**是它的命名空间，因为vuex最后编译，会把所有的action、mutation 和 getter放到一个里，这样会有重复命名的可能，如果加了这个属性，它会自动根据注册路径调整命名。这样就不用担心重复命名啦

然后在主store里就这样写：

```js
// store/index.js
import second from './second'
const store = new Vuex.Store({
    modules: {
        second
    }
})
```

好了，页面里的写法更简单，和以前基本一样

```html
<!-- page.vue -->
<template>
    <div>
        <Header :activeIndex="activeIndex"></Header>
        <div>{{count}}</div>
        <button @click="change">按我加1</button>
    </div>
</template>
<script>
import Header from "components/Header/Header.vue";
import { mapState, mapMutations} from 'vuex';
export default {
    data:() => ({
        activeIndex:'/page',
    }),
    components: {
        Header
    },
    computed: {
        ...mapState('second',{
                count: state => state.count,
        }),
    },
    methods: {
        ...mapMutations('second',{
            change: 'increment'
        }),
    }
};
</script>
```

唯一和之前的区别，就是mapState等这些函数，第一个参数就是你在store里modules里注册的属性名。

当然 ，最后看下效果，嗯，点击加1，效果不错。
![avatar](/images/vuex/vuex_demo6.png)

## 总结

看到这里，我相信你对vuex基本也有了了解。它的功能强大，增删改查一个不差。而且对于页面来说，数据简单清晰。

当然它也有缺点，即它的数据都是存在内存中的，刷新就会消失。

网上有一种方法是将它获取到的数据存入localstorage内，这样就不用担心数据问题了。当然这是一种办法，如果有这方面需求的可以尝试存一下。

余下还有问题的话，留着给以后吧。前方还有更多的东西等着我去探索。

不说了，vuex就暂时到这里吧~~~。么么哒，能看到这里的亲们，一定是个gay。

