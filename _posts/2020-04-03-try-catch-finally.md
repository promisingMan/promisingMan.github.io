---
layout:     post
title:      "浅谈try-catch-finally"
subtitle:   ""
date:       2020-04-03 13:00:00
author:     "ZengJia"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Java
---

## 废话不多说直接上代码
  
![返回值1](/img/code/try-catch-finally1.jpg)

finally总是最后执行所以返回1



## 再来看一个
![返回值0](/img/code/try-catch-finally2.jpg)

虽然finally已经将value置为1，但是结果还是返回了0，因为在catch语句块里，保存了value的副本，执行完finally语句块还是会返回当时保存的副本
