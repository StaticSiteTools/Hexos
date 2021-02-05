---
title: Test：图表
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
abbrlink: 346568510
date: 2020-07-23 01:17:12
updated: 2020-07-23 01:17:12
---


# Test:图表



## flowchart

Plugin: https://github.com/bubkoo/hexo-filter-flowchart

生成**流程图**。 

```
\`\`\`flow
st=>start: Start|past:>http://www.google.com[blank]
e=>end: End:>http://www.google.com
op1=>operation: My Operation|past
op2=>operation: Stuff|current
sub1=>subroutine: My Subroutine|invalid
cond=>condition: Yes
or No?|approved:>http://www.google.com
c2=>condition: Good idea|rejected
io=>inputoutput: catch something...|request

st->op1(right)->cond
cond(yes, right)->c2
cond(no)->sub1(left)->op1
c2(yes)->io->e
c2(no)->op2->e
\`\`\`
```



```flow
st=>start: Start|past:>http://www.google.com[blank]
e=>end: End:>http://www.google.com
op1=>operation: My Operation|past
op2=>operation: Stuff|current
sub1=>subroutine: My Subroutine|invalid
cond=>condition: Yes
or No?|approved:>http://www.google.com
c2=>condition: Good idea|rejected
io=>inputoutput: catch something...|request

st->op1(right)->cond
cond(yes, right)->c2
cond(no)->sub1(left)->op1
c2(yes)->io->e
c2(no)->op2->e
```


## sequence

Plugin: https://github.com/bubkoo/hexo-filter-sequence

生成**UML序列图**。 

```
\`\`\`sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
\`\`\`
```
```sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```





## 主题标签

### Next> Mermaid

> [Next> Mermaid-doc](https://github.com/theme-next/theme-next.org/blob/source/source/docs/tag-plugins/mermaid.md)

从文本生成图表和流程图 

```
{% mermaid type %}
... ...
{% endmermaid %}
```

#### 流程图

> [doc](https://mermaid-js.github.io/mermaid/#/flowchart)  

**从上到下**

```
{% mermaid graph TB %}
    Start --> Stop
{% endmermaid %}
```

{% mermaid graph TB %}
    Start --> Stop
{% endmermaid %}



**从左到右**

```
{% mermaid graph LR %}
    Start --> Stop
{% endmermaid %}
```

{% mermaid graph LR %}
    Start --> Stop
{% endmermaid %}



#### 顺序图

> [doc](https://mermaid-js.github.io/mermaid/#/sequenceDiagram)

```
{% mermaid sequenceDiagram %}
    Alice->>John: Hello John, how are you?
    John-->>Alice: Great!
{% endmermaid %}
```

{% mermaid sequenceDiagram %}
    Alice->>John: Hello John, how are you?
    John-->>Alice: Great!
{% endmermaid %}



#### 类图

>[doc](https://mermaid-js.github.io/mermaid/#/classDiagram)

```
{% mermaid classDiagram %}
      Animal <|-- Duck
      Animal <|-- Fish
      Animal <|-- Zebra
      Animal : +int age
      Animal : +String gender
      Animal: +isMammal()
      Animal: +mate()
      class Duck{
          +String beakColor
          +swim()
          +quack()
      }
      class Fish{
          -int sizeInFeet
          -canEat()
      }
      class Zebra{
          +bool is_wild
          +run()
      }
{% endmermaid %}
```

{% mermaid classDiagram %}
      Animal <|-- Duck
      Animal <|-- Fish
      Animal <|-- Zebra
      Animal : +int age
      Animal : +String gender
      Animal: +isMammal()
      Animal: +mate()
      class Duck{
          +String beakColor
          +swim()
          +quack()
      }
      class Fish{
          -int sizeInFeet
          -canEat()
      }
      class Zebra{
          +bool is_wild
          +run()
      }
{% endmermaid %}



#### 状态图

> [doc](https://github.com/mermaid-js/mermaid/blob/develop/docs/stateDiagram.md)

```
{% mermaid stateDiagram-v2 %}
    [*] --> First
    state First {
        [*] --> second
        second --> [*]
    }
{% endmermaid %}
```

{% mermaid stateDiagram-v2 %}
    [*] --> First
    state First {
        [*] --> second
        second --> [*]
    }
{% endmermaid %}



#### 甘特图

> [doc](https://mermaid-js.github.io/mermaid/#/gantt)

```
{% mermaid gantt %}
    title A Gantt Diagram
    dateFormat  YYYY-MM-DD
    section Section
    A task           :a1, 2014-01-01, 30d
    Another task     :after a1  , 20d
    section Another
    Task in sec      :2014-01-12  , 12d
    another task     : 24d
{% endmermaid %}
```

{% mermaid gantt %}
    title A Gantt Diagram
    dateFormat  YYYY-MM-DD
    section Section
    A task           :a1, 2014-01-01, 30d
    Another task     :after a1  , 20d
    section Another
    Task in sec      :2014-01-12  , 12d
    another task     : 24d
{% endmermaid %}



#### 饼形图

> [doc](https://mermaid-js.github.io/mermaid/#/pie)

```
{% mermaid pie title Pets adopted by volunteers %}
    "Dogs" : 386
    "Cats" : 85
    "Rats" : 15 
{% endmermaid %}
```

{% mermaid pie title Pets adopted by volunteers %}
    "Dogs" : 386
    "Cats" : 85
    "Rats" : 15 
{% endmermaid %}




## 参考阅读


