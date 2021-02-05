---
title: Test：常规测试
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
abbrlink: 4281881188
date: 2020-07-23 01:17:12
updated: 2020-07-23 01:17:12
---


# Test：常规测试

<!-- more -->

### 路径
有无前缀斜杠(`/`)的区别
```
[PDF1](Test/1.png )   #位于当前路径下
[PDF2](/images/avatar.png)   #位于站点根目录下
```
[PDF1](Test/1.png )

[PDF2](/images/avatar.png)



### Front-matter

**置顶** 

```
top: 9999
```

**不包含在搜索数据中**

```
indexing: false
```

**不包含在站点地图中**

```
sitemap: false
```



### 数学公式支持
```mathematica
$$
a = b + c
$$
```
$$
a = b + c
$$

```mathematica
$$
\frac{\partial u}{\partial t}
= h^2 \left( \frac{\partial^2 u}{\partial x^2} +
\frac{\partial^2 u}{\partial y^2} +
\frac{\partial^2 u}{\partial z^2}\right)
$$
```
$$
\frac{\partial u}{\partial t}
= h^2 \left( \frac{\partial^2 u}{\partial x^2} +
\frac{\partial^2 u}{\partial y^2} +
\frac{\partial^2 u}{\partial z^2}\right)
$$







### 嵌入式PDF

```
{% pdf  Test/1.pdf  %} 
```

{% pdf  Test/1.pdf  %} 



### 信息卡片

```
{% valkyrurl [key=value] %}

#示例
{% valkyrurl
[url=https://www.baidu.com/]
[title="百度"]
[avatar=/images/avatar.png]
[desc="百度…….."]
%}
```

{% valkyrurl
[url=https://www.baidu.com/]
[title="百度"]
[avatar=/images/avatar.png]
[desc="百度…….."]
%}



### Github信息卡片

```
{% githubCard user:UserName [key=value] %}
```

{% githubCard user:KumaNNN %}


{% githubCard user:KumaNNN  repo:KumaNNN.github.io %}





### 专题页面

[bilibili](/bangumis/index.html)



### 内容块

```
!!!  note  noteTitle
    notenotenotenotenotenotenote
```

!!!  note  noteTitle
    notenotenotenotenotenotenote



```
!!! info   infoTitle
    infoinfoinfoinfoinfoinfoinfoinfo
```

!!! info   infoTitle
    infoinfoinfoinfoinfoinfoinfoinfo




```
!!! warning  warningTitle 
    warningwarningwarningwarningwarningwarning
```

!!!  warning   warningTitle 
    warningwarningwarningwarningwarningwarning



```
!!! error  errorTitle 
    errorerrorerrorerrorerrorerrorerrorerror
```

!!!  error  errorTitle 
    errorerrorerrorerrorerrorerrorerrorerror



### 图像

#### Next> group-pictures

```
{% grouppicture 6-3 %}
  ![](/images/avatar.png)
  ![](/images/avatar2.jpg)
  ![](/images/avatar3.jpg)
  ![](/images/avatar2.jpg)
  ![](/images/avatar.png)
  ![](/images/avatar3.jpg)
{% endgrouppicture %}
```

{% grouppicture 6-2 %}
  ![](/images/avatar.png)
  ![](/images/avatar2.jpg)
  ![](/images/avatar3.jpg)
  ![](/images/avatar2.jpg)
  ![](/images/avatar.png)
  ![](/images/avatar3.jpg)
{% endgrouppicture %}





### 主题标签

#### Next> Tabs 

```
{% tabs First unique name %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}
```

{% tabs First unique name %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}



#### Next> Note

```
{% note %}
#### Header
(不定义类样式)
{% endnote %}
```

{% note %}

#### Header

(不定义类样式)
{% endnote %}



```
{% note default %}
#### Default Header
Welcome to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note default %}

#### Default Header

Welcome to [Hexo!](https://hexo.io)
{% endnote %}



```
{% note primary %}
#### Primary Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note primary %}

#### Primary Header

**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}



```
{% note info %}
#### Info Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note info %}

#### Info Header

**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}



```
{% note info no-icon %}
#### No icon note
Note **without** icon: `note info no-icon`
{% endnote %}
```

{% note info no-icon %}

#### No icon note

Note **without** icon: `note info no-icon`
{% endnote %}



```
{% note success %}
#### Success Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note success %}

#### Success Header

**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}



```
{% note warning %}
#### Warning Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note warning %}

#### Warning Header

**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}



```
{% note danger %}
#### Danger Header
**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}
```

{% note danger %}

#### Danger Header

**Welcome** to [Hexo!](https://hexo.io)
{% endnote %}





### 二维码生成

```
{% qrcode "hello, world" alt:"hello, world" title:"hello, world" hello world %}
```

{% qrcode "hello, world" alt:"hello, world" title:"hello, world" hello world %}





### 工具提示

**脚注提示**

basic footnote[^1]

and another one[^3]

and another one[^4]

[^1]: basic footnote content
[^3]: paragraph
[^4]: footnote content with some [markdown](https://en.wikipedia.org/wiki/Markdown)






## 参考阅读


