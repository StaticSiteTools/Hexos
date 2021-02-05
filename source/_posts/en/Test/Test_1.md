---
title: Test：General test
mathjax: true
typora-root-url: Test
typora-copy-images-to: Test
top: 1
comments: false
indexing: false
sitemap: false
categories:
  - en
  - Test
tags:
  - test
description: Plug in and syntax testing
abbrlink: 4281881188
date: 2020-07-23 01:17:12
updated: 2020-07-23 01:17:12
---


# Test：General test

<!-- more -->

### Path
With or without prefix slash(`/`)
```
[PDF1](Test/1.png )   #Under current path
[PDF2](/images/avatar.png)   #Under the site root directory
```
[PDF1](Test/1.png )

[PDF2](/images/avatar.png)



### Front-matter

**Topping** 

```
top: 9999
```

**Not included in search data**

```
indexing: false
```

**Not included in site map**

```
sitemap: false
```



### Mathematical formula support
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







### Embed pdf

```
{% pdf  Test/1.pdf  %} 
```

{% pdf  Test/1.pdf  %} 



### Information card

```
{% valkyrurl [key=value] %}

#Example
{% valkyrurl
[url=https://www.baidu.com/]
[title="Baidu"]
[avatar=/images/avatar.png]
[desc="Baidu…….."]
%}
```

{% valkyrurl
[url=https://www.baidu.com/]
[title="Baidu"]
[avatar=/images/avatar.png]
[desc="Baidu…….."]
%}



### Github information card

```
{% githubCard user:UserName [key=value] %}
```

{% githubCard user:KumaNNN %}


{% githubCard user:KumaNNN  repo:KumaNNN.github.io %}





### Special page

[bilibili](/bangumis/index.html)



### Content block

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



### Image

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





### Theme Tags

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
(Class styles are not defined)
{% endnote %}
```

{% note %}

#### Header

(Class styles are not defined)
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





### QR code generate

```
{% qrcode "hello, world" alt:"hello, world" title:"hello, world" hello world %}
```

{% qrcode "hello, world" alt:"hello, world" title:"hello, world" hello world %}





### Tooltip

**Footnotes**

basic footnote[^1]

and another one[^3]

and another one[^4]

[^1]: basic footnote content
[^3]: paragraph
[^4]: footnote content with some [markdown](https://en.wikipedia.org/wiki/Markdown)






## Reference reading


