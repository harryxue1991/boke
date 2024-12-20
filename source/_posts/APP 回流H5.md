---
title: APP 回流 H5
date: 2023-05-02 10:36:16
tags:
---

方案一：
1.使用URL scheme 协议唤起app
a.URL scheme 方式是一种比较通用的技术，它是由 协议、路径、参数组成。
b.打开方式： 直接通过window.location.href 跳转。或者直接用a标签
缺点：在浏览器中，无法判断是否已经安装了app
解决方案：
一般都是尝试唤起，如果安装了app，浏览器就回弹出一个toast弹窗，问你是否跳app。
如果用户点了允许，就会离开浏览器，进入到app。
所以解决无法判断是否安装了app，则通过2s延时的判断，一般超过2s没有调走，默认没有安装app，则跳转到Apple Store 或者下载安卓的apk包
<!-- more -->
<button @click="handelDownload"></button>

<script setup>

const handelDownload = () => {
    const universalLink = 'xxx universalLink 链接'
    if (isIos())) {
        window.location.href = universalLink    
    } else {
        window.location.href = myScheme.value    
    }
    
    let flag = true
    const listener = () => {
        if (download.hidden) {
            flag = false        
        }    
    }
    document.addEventListener('visibilitychange', listener)
    setTimeout(() => {
        if (flag) {
            // apk 的包
            window.location.href = 'xxxxxx'
        }
        document.removeEventListener('visibilitychange', listener)
    }, 2000)
}

</script>

方案二：
1.使用universalLink唤起
a.ios9新增的功能，通过https协议链接来打开app
b.原理：
i.在app中注册自己要支持的域名
ii.在自己的域名根目录配置一个 apple-app-site-association 文件
c.打开方式： 也是通过 window.loction.href 跳转。
缺点：如果没有安装app，则点击的无反应
解决方案：
点击无反应，所以最好的解决办法，就是在网关做重定向。
这块其实也有更好的方案，就是起一个node服务，让网关打到这个服务里，方便我们后面对这个appleStore的链接做扩展。


其他问题点：
a.在微信，微博等一些应用中，无法使用universalLink 和 scheme跳转。
b.所以在不同的app中，需要做不同的处理
在微信中：
想使用开放标签，前提必须有服务号，没有服务号就无法使用开放标签。
1.通过微信开放平台： https://open.weixin.qq.com/
2.登陆开放平台的账号，找到自己开放平台的服务号。
3.开放平台需要创建移动应用，还有关联公众号。基本上所有的操作都需要在这里配置好

4.服务号的可信任域名，关联的移动应用appid（下面开放标签中的appid）都需要一致。
5.服务号的设置，就是最基本的设置，开发者需要做的验证安全域名。

微信开放标签组件封装如下参考：
开放标签想用的好，其实有个好的思路，就是不需要在开放标签中写内容，直接是一个dom 的透明蒙层，盖在按钮上面。当在微信环境中，讲开放标签拿出来盖在上面就ok了。
<template>
  <!-- ios 使用 universal Link -->
  <wx-open-launch-app
    v-if="isWeixin() && !isIos()"
    appid="wxxxxxxxxxxxxxxxx"
    :extinfo="extinfo"
    class="wx-open-app-btn"
    @error="error"
    @ready="ready"
    @launch="launch"
  >
    <component
      :is="'script'"
      type="text/wxtag-template"
    >
      <component :is="'style'">.template { position: absolute; top: 0; left: 0; right: 0; bottom: 0;}</component>
      <div class="template"></div>
    </component>
  </wx-open-launch-app>
  <jet-lunch-app-middle-popup v-model:showModel="showModel"></jet-lunch-app-middle-popup>
</template>

<script setup>
import { ref } from 'vue';
import utils, { isWeixin, isIos } from '@/utils/index';

const showModel = ref(false);

// 从父组件传入的一个scheme地址。跟app端做好协议。
defineProps({
  extinfo: {
    type: String,
    default: () => `AilPay://app/navi?para=${encodeURIComponent(JSON.stringify({ page: 'Discover' }))}`,
  },
});
// 唤起失败逻辑
const error = () => {
  // 微信打开失败，安卓弹窗提示去下载，需要在别的浏览器打开。
  showModel.value = true;
};

// 开放标签准备完成
const ready = () => {
  console.log('ready');
};

// 唤起事件
const launch = (e) => {
  console.log(e.detail)
};
</script>

<style>
.wx-open-app-btn {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  z-index: 20;
  overflow: hidden;
}
</style>
