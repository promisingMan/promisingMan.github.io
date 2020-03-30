---
layout:     post
title:      "浅谈前端优化"
subtitle:   " \"每天积累一点之前端优化\""
date:       2020-03-28 12:43:24
author:     "ZH"
header-img: "img/home-bg-art.jpg"
catalog: true
tags:
    - 前端
---

## 工具方面实现
1. 减少请求资源大小或者次数
- 尽量合并和压缩css和js文件   
原因:减少HTTP请求次数以及请求资源大小   
办法:采用打包工具webPack配置loader或者plugins来达到压缩目的   
想了解[webPack]()如何优化可以点击查看详情配置
- 尽量采用字体图片或者[svg]()图标来代替传统png图   
原因:因为字体图标或者svg图片是用代码编写出来的，放大不会变形而且渲染快
- 采用图片懒加载   
目的为了，减少页面第一次加载过程中HTTP请求次数   
详情查看[如何实现图片懒加载]()
-  能用css做的效果，不要用js做，能用原生js做的，不要轻易去使用第三方插件。   
原因:避免引入第三方大量的库。而自己却只是用里面的一个小功能
- 使用[雪碧图片]()或者说[图片精灵]()
- 减少对cookie的使用   
目的: 较少本地cookie存储内容大小
原因: 客户端操作cookie的时候，这些信息总是在客户端和服务端传递。如果设置不当，每次发送一个请求将会携带cookie
- 前后端进行数据交换时,对于多项数据尽可能基于json格式来进行传送，这样能使数据方便，资源偏小
- 前端与后端协商，合理使用[keep-alive]()

## 代码优化相关
- 在js中尽量减少使用闭包
原因:使用闭包因为引用相互关联，会导致上下文不会释放
- 减少对dom的操作，主要是减少[dom]()的重绘以及回流  
关于回流的分离读写:如果需要设置多个样式，把设置样式全放在一起，使用文档碎片或者字符串拼接做数据绑定(dom的动态创建)
- 在js避免嵌套循环和"死循环"
- 减少[css表达式]()使用
- 尽量将一个动画元素单独设置为一个图层（避免重绘或者回流的大小）
注意：图层不要过多设置，否则不但效果没有达到反而更差了
- 在js封装过程中，尽量做到低耦合高内聚，减少页面的冗余代码
- css中设置定位时，尽量使用z-index改变盒子的层级，让盒子不在相同的平面上
- css导入的时候尽量减少@import导入式，因为@import是同步操作，只有把对应的样式导入后，才会继续向下加载，而link是异步的操作
- 使用window.requestAnimationFrame(js的帧动画)代替传统的定时器动画   
如果想使用每隔一段时间执行动画，应该避免使用setInterval，尽量使用setTimeout代替setInterval定时器。因为setInterval定时器存在弊端：可能造成两个动画间隔时间缩短
- 尽量减少使用递归，避免死递归
- 在时间绑定中，尽可能使用[事件委托]()，减少循环给dom元素绑定事件处理函数

## 存储
- 结合后端，利用浏览器的缓存技术，做一些缓存（让后端返回304，告诉浏览器去本地拉取数据）。（注意：也有弊端）可以让一些不太会改变的静态资源做缓存。比如：一些图片，js，cs
- 利用h5的新特性（localStorage、sessionStorage）做一些简单数据的存储，避免向后台请求数据或者说在离线状态下做一些数据展示

## 其他优化
- 避免使用[iframe]()不仅不好管控样式，而且相当于在本页面又嵌套其他页面，消耗性能会更大。因为还会去加载这个嵌套页面的资源
- 页面中的是数据获取采用异步编程和延迟分批加载，使用异步加载是数据主要是为了避免浏览器失去响应。如果你使用同步，加载数据很大并且很慢
那么，页面会在一段时间内处于阻塞状态。目的：为了解决请求数据不耽搁渲染，提高页面的渲染效率 。
解决方法：需要动态绑定的是数据区域先隐藏，等数据返回并且绑定后在让其显示   
延迟分批加载类似图片懒加载。减少第一次页面加载时候的http请求次数
- 页面中出现音视频标签，我们不让页面加载的时候去加载这些资源（否则第一次加载会很慢）
解决方法：只需要将音视频的preload=none即可。