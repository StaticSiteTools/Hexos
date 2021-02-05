---
title: Test：Chart
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
abbrlink: 346568510
date: 2020-07-23 01:17:12
updated: 2020-07-23 01:17:12
---


# Test: Chart



## flowchart

Plugin: https://github.com/bubkoo/hexo-filter-flowchart

Generate **flowchart**。 

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

Generate **UML sequence diagrams**。 

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





## Theme Tags

### Next> Mermaid

> [Next> Mermaid-doc](https://github.com/theme-next/theme-next.org/blob/source/source/docs/tag-plugins/mermaid.md)

Generate diagrams and flowcharts from text 

```
{% mermaid type %}
... ...
{% endmermaid %}
```

#### Flowcharts

> [doc](https://mermaid-js.github.io/mermaid/#/flowchart)  

**Top to bottom**

```
{% mermaid graph TB %}
    Start --> Stop
{% endmermaid %}
```

{% mermaid graph TB %}
    Start --> Stop
{% endmermaid %}



**Left to right**

```
{% mermaid graph LR %}
    Start --> Stop
{% endmermaid %}
```

{% mermaid graph LR %}
    Start --> Stop
{% endmermaid %}



#### Sequence Diagram

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



#### Class Diagram

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



#### State Diagram

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



#### Gantt

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



#### Pie

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




## Reference reading


