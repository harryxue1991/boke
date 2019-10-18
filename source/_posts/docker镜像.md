---
title: docker镜像
date: 2019-06-20 22:23:19
tags:
---

第一次安装完docker，想试着安装一个镜像，但是总是下载失败。

然后百度找了很多镜像都不好使，最后还是自己在阿里云镜像服务找到了办法。

首先进入阿里云镜像加速页面

<!-- more -->

地址：<https://cr.console.aliyun.com/#/accelerator>

创建自己都加速地址，复制

然后在Docker的Preferences中，选择Daemon，Register mirrors中添加复制的地址。

![avatar](/images/docker/docker_1.png)

点击apply & Restart

最后点击上面的reset。重启了docker，最后成功下载镜像