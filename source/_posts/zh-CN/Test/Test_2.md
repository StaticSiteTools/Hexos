---
title: Test：代码演示
mathjax: true
typora-root-url: Test
typora-copy-images-to: Test
top: 5
comments: false
indexing: false
sitemap: false
categories:
  - zh-CN
  - Test
tags:
  - test
description: 语法和插件等项目测试
abbrlink: 3815140717
date: 2020-07-23 01:17:12
updated: 2020-07-23 01:17:12
---


# Test:代码演示

```html
{% demo hexo-tag-demo %}
<intro>
An example for hexo-tag-demo.

The `<intro>` tag supports __markdown__.
</intro>

<template>
  <div id="colorbox"></div>
  <button id="demo-button">Click Me</button>
</template>

<script>
  document.getElementById('demo-button').onclick = function() {
    var randomColor = '#' + Math.random().toString().substr(2,6);
    document.getElementById('colorbox').innerHTML = randomColor;
    document.getElementById('colorbox').style.background = randomColor;
  }
</script>

<style>
  #colorbox {
    border: 1px solid #ddd;
    height: 150px;
    width: 200px;
    line-height: 150px;
    text-align: center;
    margin-bottom: 20px;
    color: #fff;
  }
  #demo-button {
    padding: 5px 10px;
  }
</style>
{% enddemo %}
```

{% demo hexo-tag-demo %}
<intro>
An example for hexo-tag-demo.

The `<intro>` tag supports __markdown__.
</intro>

<template>
  <div id="colorbox"></div>
  <button id="demo-button">Click Me</button>
</template>

<script>
  document.getElementById('demo-button').onclick = function() {
    var randomColor = '#' + Math.random().toString().substr(2,6);
    document.getElementById('colorbox').innerHTML = randomColor;
    document.getElementById('colorbox').style.background = randomColor;
  }
</script>

<style>
  #colorbox {
    border: 1px solid #ddd;
    height: 150px;
    width: 200px;
    line-height: 150px;
    text-align: center;
    margin-bottom: 20px;
    color: #fff;
  }
  #demo-button {
    padding: 5px 10px;
  }
</style>
{% enddemo %}




## 参考阅读


