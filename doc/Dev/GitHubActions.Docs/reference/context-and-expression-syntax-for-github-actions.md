---
title: GitHub Actions的上下文和表达语法
date: 2020-11-09  7:12:38
updated: 2020-11-09  7:12:38
mathjax: false
categories: 
tags:
typora-root-url: context-and-expression-syntax-for-github-actions
typora-copy-images-to: context-and-expression-syntax-for-github-actions
top: 1
comments: false
---


# GitHub Actions的上下文和表达语法

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)

> 您可以访问上下文信息并评估工作流和动作中的表达式。 



## 关于上下文和表达式

您可以使用表达式以编程方式在工作流文件中设置变量并访问上下文。表达式可以是文字值，对上下文的引用或函数的任意组合。您可以使用运算符组合文字，上下文引用和函数。

表达式通常与工作流文件中的条件关键字`if`一起使用，以确定是否应运行步骤。当`if`条件为时`true`，该步骤将运行。

您需要使用特定的语法来告诉GitHub评估表达式而不是将其视为字符串。

`${{ <expression> }}`

在表达式中使用`if`条件 时，您可以省略表达式语法（`${{ }}`），因为GitHub会自动将`if`条件视为表达式。有关`if`条件的更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions/#jobsjob_idif)”。

### `if`条件表达式的示例

```yaml
steps:
  - uses: actions/hello-world-javascript-action@v1.1
    if: ${{ <expression> }}
```

### 设置环境变量的示例

```yaml
env:
  my_env_var: ${{ <expression> }}
```

## 上下文

上下文是一种访问有关工作流程运行，运行器环境，作业和步骤等信息的方法。上下文使用表达式语法。

`${{ <context> }}`

| 上下文     | 类型     | 描述                                                         |
| ---------- | -------- | ------------------------------------------------------------ |
| `github`   | `object` | 有关工作流程运行的信息。有关更多信息，请参见[`github`context](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#github-context)。 |
| `env`      | `object` | 包含在工作流，作业或步骤中设置的环境变量。有关更多信息，请参见[`env`context](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#env-context)。 |
| `job`      | `object` | 有关当前正在执行的作业的信息。有关更多信息，请参见[`job`context](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#job-context)。 |
| `steps`    | `object` | 有关此作业中已运行步骤的信息。有关更多信息，请参见[`steps`context](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#steps-context)。 |
| `runner`   | `object` | 有关正在运行当前作业的跑步者的信息。有关更多信息，请参见[`runner`context](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#runner-context)。 |
| `secrets`  | `object` | 启用对机密的访问。有关机密的更多信息，请参阅“[创建和使用加密的机密”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)。 |
| `strategy` | `object` | 允许访问已配置的策略参数和有关当前作业的信息。策略参数包括`fail-fast`，`job-index`，`job-total`，和`max-parallel`。 |
| `matrix`   | `object` | 允许访问您为当前作业配置的矩阵参数。例如，如果使用`os`和`node`版本配置矩阵构建，则`matrix`上下文对象包括当前作业的`os`和`node`版本。 |
| `needs`    | `object` | 允许访问定义为当前作业依赖项的所有作业的输出。有关更多信息，请参见[`needs`context](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#needs-context)。 |

作为表达式的一部分，您可以使用两种语法之一访问上下文信息。

* 索引语法： `github['sha']`
* 属性引用语法： `github.sha`

为了使用属性引用语法，属性名称必须：

* 以`a-Z`或`_`开头。
* 后跟`a-Z` `0-9` `-`或`_`。

### `github` context

该`github`上下文包含有关工作流运行和触发运行事件的信息。您可以在环境变量中读取`github`大多数上下文数据。有关环境变量的更多信息，请参见“[使用环境变量”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/using-environment-variables)。

> **警告：**使用整个`github`上下文时，请注意其中包含敏感信息，例如`github.token`。GitHub在将其打印到控制台时会掩盖机密，但在导出或打印上下文时应谨慎。

| 属性名称                  | 类型     | 描述                                                         |
| ------------------------- | -------- | ------------------------------------------------------------ |
| `github`                  | `object` | 在工作流程的任何作业或步骤中可用的顶级上下文。               |
| `github.action`           | `string` | 当前正在运行的动作的名称。当前步骤运行脚本时，GitHub会移除特殊字符或使用名称`run`。如果在同一作业中多次使用同一动作，则该名称将包含带有序列号的后缀。例如，您运行的第一个脚本将具有名称`run1`，第二个脚本将被命名`run2`。类似地，第二次调用`actions/checkout`会是`actionscheckout2`。 |
| `github.action_path`      | `string` | 您的动作所在的路径。您可以使用此路径轻松访问与动作位于同一存储库中的文件。仅在复合运行步骤动作中支持此属性。 |
| `github.actor`            | `string` | 启动工作流程的用户的登录名。                                 |
| `github.base_ref`         | `string` | 工作流运行中的拉取请求的目标分支或`base_ref`。仅当触发工作流程运行的事件为`pull_request`时，此属性才可用。 |
| `github.event`            | `object` | 完整的事件Webhook负载。有关更多信息，请参阅“[触发工作流的事件](https://docs.github.com/en/free-pro-team@latest/articles/events-that-trigger-workflows)”。您可以使用此上下文访问事件的各个属性。 |
| `github.event_name`       | `string` | 触发工作流运行的事件的名称。                                 |
| `github.event_path`       | `string` | 运行程序上完整事件Webhook负载的路径。                        |
| `github.head_ref`         | `string` | 工作流运行中拉取请求的源分支或`head_ref`。仅当触发工作流程运行的事件为`pull_request`时，此属性才可用。 |
| `github.job`              | `string` | 当前作业的[`job_id`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_id)。 |
| `github.ref`              | `string` | 触发工作流程运行的分支或标记引用。对于分支来说是格式 `refs/heads/<branch_name>`，对于标签来说是`refs/tags/<tag_name>`。 |
| `github.repository`       | `string` | 所有者和存储库名称。例如，`Codertocat/Hello-World`。         |
| `github.repository_owner` | `string` | 存储库所有者的名称。例如，`Codertocat`。                     |
| `github.run_id`           | `string` | 存储库中每次运行的唯一编号。如果您重新运行工作流运行，则此数字不会更改。 |
| `github.run_number`       | `string` | 存储库中特定工作流程的每次运行的唯一编号。对于工作流程的第一次运行，此数字从1开始，并在每次新运行时递增。如果您重新运行工作流运行，则此数字不会更改。 |
| `github.sha`              | `string` | 触发工作流程运行的提交SHA。                                  |
| `github.token`            | `string` | 用于代表存储库中安装的GitHub App进行身份验证的令牌。这在功能上等同于`GITHUB_TOKEN ` 机密。有关更多信息，请参见“[使用GITHUB_TOKEN进行身份验证](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/authenticating-with-the-github_token)”。 |
| `github.workflow`         | `string` | 工作流程的名称。如果工作流文件未指定`name`，则此属性的值是工作流文件在存储库中的完整路径。 |
| `github.workspace`        | `string` | 使用[`checkout`](https://github.com/actions/checkout)动作时，步骤的默认工作目录和存储库的默认位置。 |

### `env` context

该`env`上下文包含已经在工作流程，工作，或步骤中设置环境变量。有关在工作流中设置环境变量的更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#env)”。

该`env`上下文语法允许您使用环境变量的值，在您的工作流程文件。如果要在运行程序中使用环境变量的值，请使用运行程序操作系统的常规方法读取环境变量。

你只能在`with`和`name`键的值中使用`env`上下文，或者在步骤的`if`条件中使用。有关步骤语法的更多信息，请参见“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idsteps)”。

| 属性名称         | 类型     | 描述                                                         |
| ---------------- | -------- | ------------------------------------------------------------ |
| `env`            | `object` | 此上下文随作业中的每个步骤而变化。您可以从作业的任何步骤访问此上下文。 |
| `env.<env name>` | `string` | 特定环境变量的值。                                           |

### `job` context

该`job`上下文包含有关当前正在运行的作业信息。

| 属性名称                            | 类型     | 描述                                                         |
| ----------------------------------- | -------- | ------------------------------------------------------------ |
| `job`                               | `object` | 对于工作流运行中的每个作业，此上下文都会更改。您可以从作业的任何步骤访问此上下文。 |
| `job.container`                     | `object` | 有关作业容器的信息。有关容器的更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#jobsjob_idcontainer)”。 |
| `job.container.id`                  | `string` | 容器的ID。                                                   |
| `job.container.network`             | `string` | 容器网络的ID。运行程序创建作业中所有容器使用的网络。         |
| `job.services`                      | `object` | 为作业创建的服务容器。有关服务容器的更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#jobsjob_idservices)”。 |
| `job.services.<service id>.id`      | `string` | 服务容器的ID。                                               |
| `job.services.<service id>.network` | `string` | 服务容器网络的ID。运行程序创建作业中所有容器使用的网络。     |
| `job.services.<service id>.ports`   | `object` | 服务容器的暴露的端口。                                       |
| `job.status`                        | `string` | 作业的当前状态。可能的值`success`，`failure`或`cancelled`。  |

### `steps` context

该`steps`上下文包含有关已经运行在当前作业步骤的信息。

| 属性名称                                | 类型     | 描述                                                         |
| --------------------------------------- | -------- | ------------------------------------------------------------ |
| `steps`                                 | `object` | 此上下文随作业中的每个步骤而变化。您可以从作业的任何步骤访问此上下文。 |
| `steps.<step id>.outputs`               | `object` | 为该步骤定义的一组输出。有关更多信息，请参阅“ [GitHub Actions的元数据语法](https://docs.github.com/en/free-pro-team@latest/articles/metadata-syntax-for-github-actions#outputs)”。 |
| `steps.<step id>.conclusion`            | `string` | 应用[`continue-on-error`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepscontinue-on-error)完成后的步骤的结果。可能的值是`success`，`failure`，`cancelled`，或`skipped`。如果某个`continue-on-error`步骤失败，`outcome`则为`failure`，但最终`conclusion`值为`success`。 |
| `steps.<step id>.outcome`               | `string` | 应用[`continue-on-error`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepscontinue-on-error)之前完成的步骤的结果。可能的值是`success`，`failure`，`cancelled`，或`skipped`。如果某个`continue-on-error`步骤失败，`outcome`则为`failure`，但最终`conclusion`值为`success`。 |
| `steps.<step id>.outputs.<output name>` | `string` | 特定输出的值。                                               |

### `runner` context

该`runner`上下文包含有关执行当前作业运行器的信息。

| 属性名称            | 类型     | 描述                                                         |
| ------------------- | -------- | ------------------------------------------------------------ |
| `runner.os`         | `string` | 运行作业的运行器的操作系统。可能的值`Linux`，`Windows`或`macOS`。 |
| `runner.temp`       | `string` | 运行程序的临时目录的路径。保证在每个作业开始时该目录都为空，即使是在自托管运行器中也是如此。 |
| `runner.tool_cache` | `string` | 目录的路径，其中包含一些针对GitHub托管的运行器的预安装工具。有关更多信息，请参见“ [GitHub托管的运行程序规范](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners/#supported-software)”。 |

### `needs` context

`needs`上下文包含定义为当前作业依赖项的所有作业的输出。有关定义作业依赖关系的更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idneeds)”。

| 属性名称                               | 类型     | 描述                                                         |
| -------------------------------------- | -------- | ------------------------------------------------------------ |
| `needs.<job id>`                       | `object` | 当前作业所依赖的单个作业。                                   |
| `needs.<job id>.outputs`               | `object` | 当前作业所依赖的作业的输出集。                               |
| `needs.<job id>.outputs.<output name>` | `string` | 当前作业所依赖的作业的特定输出值。                           |
| `needs.<job id>.result`                | `string` | 当前作业所依赖的作业的结果。 可能的值是`success`，`failure`，`cancelled`，或`skipped`。 |

### 将上下文信息打印到日志文件的示例

要检查每个上下文中可访问的信息，可以使用此工作流程文件示例。

> **警告：**使用整个`github`上下文时，请注意其中包含敏感信息，例如`github.token`。GitHub在将其打印到控制台时会掩盖机密，但在导出或打印上下文时应谨慎。

**.github / workflows / main.yml**

```yaml
on: push

jobs:
  one:
    runs-on: ubuntu-latest
    steps:
      - name: Dump GitHub context
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: echo "$GITHUB_CONTEXT"
      - name: Dump job context
        env:
          JOB_CONTEXT: ${{ toJson(job) }}
        run: echo "$JOB_CONTEXT"
      - name: Dump steps context
        env:
          STEPS_CONTEXT: ${{ toJson(steps) }}
        run: echo "$STEPS_CONTEXT"
      - name: Dump runner context
        env:
          RUNNER_CONTEXT: ${{ toJson(runner) }}
        run: echo "$RUNNER_CONTEXT"
      - name: Dump strategy context
        env:
          STRATEGY_CONTEXT: ${{ toJson(strategy) }}
        run: echo "$STRATEGY_CONTEXT"
      - name: Dump matrix context
        env:
          MATRIX_CONTEXT: ${{ toJson(matrix) }}
        run: echo "$MATRIX_CONTEXT"
```

## 直接常量

作为表达式的一部分，你可以使用`boolean`，`null`，`number`，或`string`数据类型。布尔文字不区分大小写，因此可以使用`true`或`True`。

| 数据类型  | 文字价值                                   |
| --------- | ------------------------------------------ |
| `boolean` | `true` 要么 `false`                        |
| `null`    | `null`                                     |
| `number`  | JSON支持的任何数字格式。                   |
| `string`  | 您必须使用单引号。用单引号转义文字单引号。 |

**示例**

```yaml
env:
  myNull: ${{ null }}
  myBoolean: ${{ false }}
  myIntegerNumber: ${{ 711 }}
  myFloatNumber: ${{ -9.2 }}
  myHexNumber: ${{ 0xff }}
  myExponentialNumber: ${{ -2.99-e2 }}
  myString: ${{ 'Mona the Octocat' }}
  myEscapedString: ${{ 'It''s open source!' }}
```

## 运算符

| 运算符 | 描述       |
| ------ | ---------- |
| `( )`  | 逻辑分组   |
| `[ ]`  | 索引       |
| `.`    | 属性引用   |
| `!`    | 取反       |
| `<`    | 小于       |
| `<=`   | 小于或等于 |
| `>`    | 大于       |
| `>=`   | 大于或等于 |
| `==`   | 等于       |
| `!=`   | 不相等     |
| `&&`   | 和         |
| `||`   | 或         |

GitHub执行松散的相等比较。

* 如果类型不匹配，则GitHub将类型强制为数字。GitHub使用以下转换将数据类型转换为数字：

  | 类型   | 结果                                                         |
  | ------ | ------------------------------------------------------------ |
  | 空值   | `0`                                                          |
  | 布尔型 | `true` 返回`1`，  `false` 返回`0`                            |
  | 字符串 | 从任何合法的JSON数字格式解析，否则`NaN`。 注意：空字符串将返回`0`。 |
  | 数组   | `NaN`                                                        |
  | 对象   | `NaN`                                                        |

* 一个`NaN`与另一个`NaN`的比较结果不会为`true`。有关更多信息，请参见“ [NaN Mozilla文档”](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)。

* GitHub比较字符串时会忽略大小写。

* 仅当对象和数组是同一实例时，才认为它们相等。

## 函数

GitHub提供了一组可在表达式中使用的内置函数。某些函数将值转换为字符串以执行比较。GitHub使用以下转换将数据类型转换为字符串：

| 类型   | 结果                   |
| ------ | ---------------------- |
| 空值   | `''`                   |
| 布尔型 | `'true'`  或 `'false'` |
| 数值   | 十进制格式，指数为大数 |
| 数组   | 数组不转换为字符串     |
| 目的   | 对象不转换为字符串     |

### contains

`contains( search, item )`

返回`true` 如果`search`包含`item`。如果`search`是数组，如果`item`是数组中的元素，则此函数返回`true`。如果`search`是字符串，如果`item`是`search`的子字符串，则此函数返回`true`。 此功能不区分大小写。将值转换为字符串。

#### 使用数组的例子

`contains(github.event.issue.labels.*.name, 'bug')`

#### 使用字符串的例子

`contains('Hello world', 'llo')` 返回 `true`

### startsWith

`startsWith( searchString, searchValue )`

当`searchString`以`searchValue`开头时返回`true`。此功能不区分大小写。将值转换为字符串。

**示例**

`startsWith('Hello world', 'He')` 返回 `true`

### endsWith

`endsWith( searchString, searchValue )`

如果`searchString`以`searchValue`结尾，则返回`true`。此功能不区分大小写。将值转换为字符串。

**示例** 

`endsWith('Hello world', 'ld')` 返回 `true`

### format

`format( string, replaceValue0, replaceValue1, ..., replaceValueN)`

将`string`的值替换为变量`replaceValueN`。`string`中的变量是使用`{N}`语法指定的，其中`N`是一个整数。 您必须至少指定一个`replaceValue`和`string`。您可以使用的变量数（`replaceValueN`）没有最大值。使用双括号转义大括号。

**示例**

返回“ Hello Mona the Octocat”

`format('Hello {0} {1} {2}', 'Mona', 'the', 'Octocat')`

#### 转义括号示例

返回“ {Hello Mona the Octocat！}”

```
format('{{Hello {0} {1} {2}!}}', 'Mona', 'the', 'Octocat')
```

### join

`join( array, optionalSeparator )`

`array`的值可以是数组或字符串。`array`中的所有值都串联成一个字符串。如果提供`optionalSeparator`，则将其插入到串联值之间。否则，将使用默认的分隔符`,`。将值转换为字符串。

**示例**

`join(github.event.issue.labels.*.name, ', ')` 可能会返回 'bug, help wanted' 

### toJson

`toJSON(value)`

返回`value`的漂亮的JSON表示形式。您可以使用此功能来调试上下文中提供的信息。

**示例**

`toJSON(job)` 可能会返回 `{ "status": "Success" }`

### fromJson

`fromJSON(value)`

返回`value`的JSON对象。您可以使用此函数提供JSON对象作为评估表达式。

**示例**

此工作流程在一个作业中设置JSON矩阵，然后使用输出和`fromJSON`将其传递到下一个作业。

```yaml
name: build
on: push
jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      matrix: ${{ steps.set-matrix.outputs.matrix }}
    steps:
    - id: set-matrix
      run: echo "::set-output name=matrix::{\"include\":[{\"project\":\"foo\",\"config\":\"Debug\"},{\"project\":\"bar\",\"config\":\"Release\"}]}"
  job2:
    needs: job1
    runs-on: ubuntu-latest
    strategy:
      matrix: ${{fromJson(needs.job1.outputs.matrix)}}
    steps:
    - run: build
```

### hashFiles

`hashFiles(path)`

返回与`path`模式匹配的文件集的单个哈希。您可以提供一个`path`或多个`path`模式，并以逗号分隔。`path`相对于`GITHUB_WORKSPACE`目录，只能包括`GITHUB_WORKSPACE`内部文件。此函数为每个匹配的文件计算一个单独的SHA-256哈希，然后使用这些哈希来为文件集计算最终的SHA-256哈希。有关SHA-256的更多信息，请参见“ [SHA-2”](https://en.wikipedia.org/wiki/SHA-2)。

您可以使用模式匹配字符来匹配文件名。模式匹配在Windows上不区分大小写。有关支持的模式匹配字符的更多信息，请参见“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions/#filter-pattern-cheat-sheet)”。

#### 单模式示例

匹配存储库中的任何`package-lock.json`文件。

`hashFiles('**/package-lock.json')`

#### 多模式示例

为存储库中的任何文件`package-lock.json`和`Gemfile.lock`文件创建哈希。

`hashFiles('**/package-lock.json', '**/Gemfile.lock')`

## 作业状态检查函数

您可以将以下状态检查函数用作`if`条件语句中的表达式。如果您的`if`表达式不包含任何状态函数，它将自动产生`success()`结果。有关`if`条件的更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions/#jobsjob_idif)”。

### success

当先前的步骤均未失败或被取消时，返回`true`。

**示例**

```yaml
steps:
  ...
  - name: The job has succeeded
    if: ${{ success() }}
```

### always

始终返回`true`，即使已取消也是如此。当严重故障阻止任务运行时，作业或步骤将不会运行。例如，如果获取源失败。

**示例**

```yaml
if: ${{ always() }}
```

### cancelled

返回`true`如果工作流是已取消。

**示例**

```yaml
if: ${{ cancelled() }}
```

### failure

当作业的任何前一步失败时返回`true`。

**示例**

```yaml
steps:
  ...
  - name: The job has failed
    if: ${{ failure() }}
```

## 对象过滤器

您可以使用`*`语法来应用过滤器并选择集合中的匹配项。

例如，过滤一个名为`fruits`的对象数组。

```json
[
  { "name": "apple", "quantity": 1 },
  { "name": "orange", "quantity": 2 },
  { "name": "pear", "quantity": 1 }
]
```

过滤器`fruits.*.name`返回数组`[ "apple", "orange", "pear" ]`



## 参考阅读


