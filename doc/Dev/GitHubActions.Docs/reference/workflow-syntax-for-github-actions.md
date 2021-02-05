---
title: GitHub Actions的工作流语法
date: 2020-11-03  7:51:34
updated: 2020-11-03  7:51:34
mathjax: false
categories: 
tags:
typora-root-url: workflow-syntax-for-github-actions
typora-copy-images-to: workflow-syntax-for-github-actions
top: 1
comments: false
---


# GitHub Actions的工作流语法

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions)

> 工作流是由一个或多个作业组成的可配置的自动化过程。您必须创建YAML文件来定义工作流配置。 



## 关于工作流程的YAML语法

工作流文件使用YAML语法，并且必须具有`.yml`或`.yaml`文件扩展名。如果您不熟悉YAML并想了解更多信息，请参阅“[五分钟内学习YAML](https://www.codeproject.com/Articles/1214409/Learn-YAML-in-five-minutes) ”。

您必须将工作流文件存储在存储库的`.github/workflows`目录中。

## `name`

工作流程的名称。GitHub在存储库的“动作”页面上显示工作流程的名称。如果省略`name`，则GitHub会将其设置为相对于存储库根目录的工作流文件路径。

## `on`

**必需** 。触发工作流的GitHub事件的名称。您可以提供单个事件`string`、事件的`array`、事件`types`的`array`或事件配置`map`，以安排工作流程或将工作流程的执行限制为特定的文件，标记或分支更改。有关可用事件的列表，请参阅“[触发工作流的事件](https://docs.github.com/en/free-pro-team@latest/articles/events-that-trigger-workflows)” 。

**使用单个事件的示例**

```yaml
# 当代码被推送到存储库中的任何分支时触发
on: push
```

**使用事件列表的示例**

```yaml
# 触发推或拉请求事件的工作流
on: [push, pull_request]
```

**使用具有活动类型或配置的多个事件的示例**

如果需要为事件指定活动类型或配置，则必须分别配置每个事件。您必须在所有事件后加上冒号（`:`），包括没有配置的事件。

```yaml
on:
  # 触发推或拉请求的工作流，但仅限于main分支
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  # 也会触发页面生成，以及发布创建的事件
  page_build:
  release:
    types: # 此配置不影响上面的页面生成事件
      - created
```

## `on.<event_name>.types`

选择将触发工作流程运行的活动类型。大多数GitHub事件是由一种以上类型的活动触发的。例如，当触发释放资源的情况下，当一个版本是`published`，`unpublished`，`created`，`edited`，`deleted`，或`prereleased`。该`types`关键字允许你缩小，导致工作流运行活动。当只有一种活动类型触发Webhook事件时，不需要关键字`types`。

您可以使用事件`types`数组。有关每个事件及其活动类型的更多信息，请参阅“[触发工作流的事件](https://docs.github.com/en/free-pro-team@latest/articles/events-that-trigger-workflows#webhook-events)”。

```yaml
# 触发拉取请求活动的工作流
on:
  release:
    # 仅使用types关键字缩小将触发工作流的活动类型。
    types: [published, created, edited]
```

## `on.<push|pull_request>.<branches|tags>`

使用`push`和`pull_request`事件时，可以将工作流配置为在特定分支或标记上运行。对于`pull_request`事件，仅评估基础上的分支和标签。如果仅定义`tags`或仅定义`branches`，则对于影响未定义的Git 引用的事件，工作流不会运行。 

在`branches`，`branches-ignore`，`tags`，和`tags-ignore`关键字接受使用glob模式`*`和`**`通配符来匹配多个分支或标记名称。有关更多信息，请参见“[过滤器模式备忘单”](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet)。

**包含分支和标签的示例**

在`branches`和`tags`中定义的模式将根据Git引用的名称进行评估。例如，定义的模式 `mona/octocat`在`branches`将匹配`refs/heads/mona/octocat`GIT引用。该模式`releases/**`将与`refs/heads/releases/10`Git引用匹配。

```yaml
on:
  push:
    # 与refs/heads匹配的模式序列
    branches:    
      # main分支上的推送事件
      - main
      # 将事件推送到与refs/heads/mona/octocat匹配的分支
      - 'mona/octocat'
      # 将事件推送到与refs/heads/release/10匹配的分支
      - 'releases/**'
    # 与refs/tags匹配的模式序列
    tags:        
      - v1             # 将事件推送到 v1 标签
      - v1.*           # 将事件推送到 v1.0, v1.1, 和 v1.9 标签
```

**忽略分支和标签的示例** 

只要模式与`branches-ignore`或`tags-ignore`模式匹配，工作流就不会运行。在`branches-ignore`和`tags-ignore`中定义的模式将根据Git引用的名称进行评估。例如，定义的模式`mona/octocat`在`branches`将匹配`refs/heads/mona/octocat`GIT引用。该模式`releases/**-alpha`在`branches`将匹配`refs/releases/beta/3-alpha`Git引用。

```yaml
on:
  push:
    # 与refs/heads匹配的模式序列
    branches-ignore:
      # 将事件推送到与refs/heads/mona/octocat匹配的分支
      - 'mona/octocat'
      # 将事件推送到与refs/heads/releases/beta/3-alpha匹配的分支
      - 'releases/**-alpha'
    # 与refs/tags匹配的模式序列
    tags-ignore:
      - v1.*           # 将事件推送到 v1.0, v1.1, 和 v1.9 标签
```

**不包括分支机构和标签**

您可以使用两种类型的过滤器来防止工作流在对标签和分支的推送和拉取请求上运行。

* `branches`或`branches-ignore` - 您不能对工作流程中的同一事件同时使用`branches`和`branches-ignore`过滤器。需要过滤正匹配的分支并排除分支时，请使用`branches`过滤器。仅在需要排除分支名称时使用`branches-ignore`过滤器。
* `tags`或`tags-ignore` - 您不能对工作流程中的同一事件同时使用`tags`和`tags-ignore`过滤器。需要过滤正匹配的标签并排除标签时，请使用`tags`过滤器。仅在需要排除标记名称时使用`tags-ignore`过滤器。

**使用正负模式的示例**

您可以排除`tags`和`branches`通过使用`!`字符。定义模式的顺序很重要。

* 正匹配之后匹配的负模式（以`!`为前缀）将排除的Git引用。
* 负匹配后匹配的正模式将再次包含的Git引用。

以下工作流将在`releases/10`或`releases/beta/mona`的推送上运行，但不会在`releases/10-alpha`或`releases/beta/3-alpha`上运行，因为否定模式`!releases/**-alpha`遵循肯定模式。

```yaml
on:
  push:
    branches:    
    - 'releases/**'
    - '!releases/**-alpha'
```

## `on.<push|pull_request>.paths`

使用`push`和`pull_request`事件时，您可以将工作流配置为在至少一个文件与`paths-ignore`不匹配或至少有一个修改的文件与配置的`paths`匹配时运行。

在`paths-ignore`和`paths`关键字接受使用glob模式`*`和`**`通配符匹配多个路径名。有关更多信息，请参见“[过滤器模式备忘单”](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet)。

**忽略路径的示例**

只要路径名称与`paths-ignore`中的模式匹配，工作流就不会运行。GitHub根据路径名评估在`paths-ignore` 中定义的模式。具有以下路径过滤器的工作流将仅在`push`事件中运行，这些事件至少包括资源库根目录下的`docs`目录之外的一个文件。

```yaml
on:
  push:
    paths-ignore:
    - 'docs/**'
```

**包含路径的示例**

如果至少一条路径与`paths`过滤器中的模式匹配，则运行工作流程。要随时推送JavaScript文件来触发构建，可以使用通配符模式。

```yaml
on:
  push:
    paths:
    - '**.js'
```

**排除路径**

您可以使用两种类型的过滤器排除路径。您不能将两个过滤器都用于工作流程中的同一事件。

* `paths-ignore`  -  仅在需要排除路径名时使用`paths-ignore`过滤器。
* `paths`  -  需要过滤正向匹配路径并排除路径时，请使用`paths`过滤器。

**使用正负模式的示例**

您可以排除`paths`通过使用`!`字符。定义模式的顺序很重要：

* 正匹配之后匹配的负模式（以`!`为前缀）将排除该路径。
* 否定匹配之后匹配的肯定模式将再次包含路径。

该示例在`push`事件在`sub-project`目录或其子目录中包含文件的任何时间运行，除非该文件在`sub-project/docs`目录中。例如，更改`sub-project/index.js`或`sub-project/src/index.js`的推送将触发工作流程运行，但仅更改`sub-project/docs/readme.md`不会触发。

```yaml
on:
  push:
    paths:
    - 'sub-project/**'
    - '!sub-project/docs/**'
```

**Git 差异比较**

> **注意：**如果您推送超过1,000次提交，或者GitHub由于超时（差异太大）而未生成差异，则工作流将始终运行。

过滤器通过评估更改的文件并针对“`paths-ignore`或”`paths`列表运行它们来确定是否应运行工作流程。如果没有文件更改，则工作流将不会运行。

GitHub使用两点差异进行推送并使用三点差异进行拉取请求来生成已更改文件的列表：

* **提取请求：**三点差异是主题分支的最新版本与主题分支与基础分支上次同步的提交之间的比较。
* **推送到现有分支：**两点差异直接比较head和基础SHA。
* **推送到新分支：**与最深提交的祖先的父代之间的两点差异。

有关更多信息，请参见“[关于比较请求中的分支](https://docs.github.com/en/free-pro-team@latest/articles/about-comparing-branches-in-pull-requests)”。

## `on.schedule`

您可以使用[POSIX cron语法](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html#tag_20_25_07)安排工作流在特定的UTC时间运行。预定的工作流在默认或基本分支上的最新提交上运行。您可以运行计划的工作流程的最短间隔是每5分钟一次。

此示例每15分钟触发一次工作流：

```yaml
on:
  schedule:
    # * 是YAML中的一个特殊字符，因此必须引号此字符串
    - cron:  '*/15 * * * *'
```

有关cron语法的更多信息，请参阅“[触发工作流的事件](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/events-that-trigger-workflows#scheduled-events)”。

## `env`

一个`map`环境变量，可用于工作流中的所有作业和步骤。您还可以设置仅对作业或步骤可用的环境变量。有关更多信息，请参见[`jobs.<job_id>.env`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idenv)和[`jobs.<job_id>.steps.env`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsenv)。

当使用同一个名称定义了多个环境变量时，GitHub使用最具体的环境变量。 例如，在执行步骤时，在步骤中定义的环境变量将覆盖具有相同名称的作业和工作流变量。在作业执行时，为作业定义的变量将覆盖具有相同名称的工作流程变量。

**示例**

```yaml
env:
  SERVER: production
```

## `defaults`

一个`map`的默认设置，适用于工作流中的所有作业。您还可以设置仅适用于作业的默认设置。有关更多信息，请参见[`jobs.<job_id>.defaults`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_iddefaults)。

当使用同一个名称定义了多个默认设置时，GitHub将使用最具体的默认设置。例如，作业中定义的默认设置将覆盖与工作流程中定义的名称相同的默认设置。

## `defaults.run`

您可以为工作流中的所有[`run`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsrun)步骤提供默认的`shell`和`working-directory`选项。您还可以设置仅适用于作业的`run`默认设置。有关更多信息，请参见[`jobs.<job_id>.defaults.run`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_iddefaultsrun)。您不能在此关键字中使用上下文或表达式。

当使用同一个名称定义了多个默认设置时，GitHub将使用最具体的默认设置。例如，作业中定义的默认设置将覆盖与工作流程中定义的名称相同的默认设置。

**示例**

```yaml
defaults:
  run:
    shell: bash
    working-directory: scripts
```

## `jobs`

工作流程运行由一个或多个作业组成。默认情况下，作业并行运行。要顺序运行作业，可以使用`jobs.<job_id>.needs`关键字定义对其他作业的依赖关系。

每个作业都在由`runs-on`指定的环境中运行。

只要您处于工作流程使用限制内，就可以运行无限数量的作业。有关更多信息，请参阅GitHub托管的运行器的“[使用限制和计费](https://docs.github.com/en/free-pro-team@latest/actions/reference/usage-limits-billing-and-administration)”，以及自托管的运行器使用限制的“[关于自托管的运行器](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners/#usage-limits)”。

如果需要查找在工作流程运行中运行的作业的唯一标识符，则可以使用GitHub API。有关更多信息，请参见“[工作流作业”](https://docs.github.com/en/free-pro-team@latest/v3/actions/workflow-jobs)。

## `jobs.<job_id>`

每个作业必须具有一个ID才能与该作业关联。键`job_id`是一个字符串，其值是作业的配置数据的映射。您必须用`jobs`对象唯一的字符串替换`<job_id>`。`<job_id>`必须以字母开头，或者`_`和只包含字母数字字符，`-`或`_`。

**示例**

```yaml
jobs:
  my_first_job:
    name: My first job
  my_second_job:
    name: My second job
```

## `jobs.<job_id>.name`

显示在GitHub上的作业名称。

## `jobs.<job_id>.needs`

标识运行此作业之前必须成功完成的所有作业。 它可以是字符串或字符串数组。如果作业失败，则将跳过所有需要该作业的作业，除非该作业使用导致该作业继续的条件语句。

**示例**

```yaml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```

在此示例中，`job1`必须在`job2`开始之前成功完成，然后`job3`等待`job1`和`job2`完成。

此示例中的作业按顺序运行：

1. `job1`
2. `job2`
3. `job3`

## `jobs.<job_id>.runs-on`

**必需**。在其上运行作业的计算机的类型。该机器可以是GitHub托管的运行程序，也可以是自托管的运行程序。

**GitHub托管的运行程序**

如果您使用GitHub托管的运行程序，则每个作业都在`runs-on`所指定的虚拟环境的新实例中运行。

GitHub托管的可用运行器类型为：

| 虚拟环境             | YAML工作流程标签                     |
| -------------------- | ------------------------------------ |
| Windows Server 2019  | `windows-latest` 要么 `windows-2019` |
| Ubuntu 20.04         | `ubuntu-20.04`                       |
| Ubuntu 18.04         | `ubuntu-latest` 要么 `ubuntu-18.04`  |
| Ubuntu 16.04         | `ubuntu-16.04`                       |
| macOS Big Sur 11.0   | `macos-11.0`                         |
| macOS Catalina 10.15 | `macos-latest` 要么 `macos-10.15`    |

> **注意：**当前仅提供Ubuntu 20.04虚拟环境的预览。该`ubuntu-latest`YAML工作流标签仍然采用了Ubuntu 18.04虚拟环境。

示例

```yaml
runs-on: ubuntu-latest
```

有关更多信息，请参阅“[面向GitHub托管的运行器的虚拟环境](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/virtual-environments-for-github-hosted-runners)”。

**自托管的运行程序**

要为您的工作指定自托管运行器，请在工作流文件中`runs-on`配置自托管运行器标签。

所有自承载流道都有`self-hosted`标签，您可以通过仅提供`self-hosted`标签来选择任何自承载流道。或者，您可以在带有其他标签的阵列中使用该`self-hosted`标签，例如特定操作系统或系统体系结构的标签，以仅选择指定的运行器类型。

示例

```
runs-on: [self-hosted, linux]
```

有关更多信息，请参阅“[关于自托管运行器](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/about-self-hosted-runners)”和“[在工作流中使用自托管运行器](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/using-self-hosted-runners-in-a-workflow)”。

## `jobs.<job_id>.outputs`

作业的`map`输出。作业输出可用于所有依赖此作业的下游作业。有关定义作业依赖性的更多信息，请参见[`jobs.<job_id>.needs`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idneeds)。

作业输出是字符串，并且包含表达式的作业输出在每个作业结束时在运行程序上进行求值。包含机密的输出会在运行程序中编辑，并且不会发送到GitHub Actions。

要在相关作业中使用作业输出，可以使用`needs`上下文。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#needs-context)”。

**示例**

```yaml
jobs:
  job1:
    runs-on: ubuntu-latest
    # 将步骤输出映射到作业输出
    outputs:
      output1: ${{ steps.step1.outputs.test }}
      output2: ${{ steps.step2.outputs.test }}
    steps:
    - id: step1
      run: echo "::set-output name=test::hello"
    - id: step2
      run: echo "::set-output name=test::world"
  job2:
    runs-on: ubuntu-latest
    needs: job1
    steps:
    - run: echo ${{needs.job1.outputs.output1}} ${{needs.job1.outputs.output2}}
```

## `jobs.<job_id>.env`

一个可用于作业中所有步骤的`map`环境变量。您还可以为整个工作流程或单个步骤设置环境变量。有关更多信息，请参见[`env`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#env)和[`jobs.<job_id>.steps.env`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsenv)。

当使用同一个名称定义了多个环境变量时，GitHub将使用最具体的环境变量。例如，在执行步骤时，在步骤中定义的环境变量将覆盖具有相同名称的作业和工作流变量。在作业执行时，为作业定义的变量将覆盖具有相同名称的工作流程变量。

**示例** 

```yaml
jobs:
  job1:
    env:
      FIRST_NAME: Mona
```

## `jobs.<job_id>.defaults`

一个`map`的默认设置，适用于作业中的所有步骤。您还可以为整个工作流程设置默认设置。有关更多信息，请参见[`defaults`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#defaults)。

当使用同一个名称定义了多个默认设置时，GitHub将使用最具体的默认设置。例如，作业中定义的默认设置将覆盖与工作流程中定义的名称相同的默认设置。

## `jobs.<job_id>.defaults.run`

为作业中的所有`run`步骤提供默认的`shell`和`working-directory`。本节中不允许上下文和表达式。

您可以为作业中的所有[`run`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsrun)步骤提供默认值`shell`和`working-directory`选项。您还可以为整个工作流程设置默认`run`设置。有关更多信息，请参见[`jobs.defaults.run`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#defaultsrun)。您不能在此关键字中使用上下文或表达式。

当使用同一个名称定义了多个默认设置时，GitHub将使用最具体的默认设置。例如，作业中定义的默认设置将覆盖与工作流程中定义的名称相同的默认设置。

**示例**

```yaml
jobs:
  job1:
    runs-on: ubuntu-latest
    defaults:
      run:
        shell: bash
        working-directory: scripts
```

## `jobs.<job_id>.if`

您可以使用`if`条件来阻止作业运行，除非满足条件。您可以使用任何受支持的上下文和表达式来创建条件。

在`if`条件表达式中使用表达式时，您可以省略表达式语法（`${{ }}`），因为GitHub会自动将`if`条件表达式视为表达式。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)”。

## `jobs.<job_id>.steps`

作业包含称为`steps`的一系列任务。步骤可以运行命令，运行设置任务，或在存储库，公共存储库中运行动作，或在Docker注册表中发布动作。并非所有步骤都运行动作，但是所有动作都作为一个步骤运行。**每个步骤都在运行器环境中以其自己的进程运行，并且可以访问工作空间和文件系统**。因为步骤在其自己的进程中运行，所以在步骤之间不会保留对环境变量的更改。GitHub提供了内置步骤来设置和完成工作。

只要您处于工作流程使用限制之内，就可以运行无限数量的步骤。有关更多信息，请参阅GitHub托管的运行器的“[使用限制和计费](https://docs.github.com/en/free-pro-team@latest/actions/reference/usage-limits-billing-and-administration)”，以及自托管的运行器使用限制的“[关于自托管的运行器](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners/#usage-limits)”。

**示例** 

```yaml
name: Greeting from Mona

on: push

jobs:
  my-job:
    name: My Job
    runs-on: ubuntu-latest
    steps:
    - name: Print a greeting
      env:
        MY_VAR: Hi there! My name is
        FIRST_NAME: Mona
        MIDDLE_NAME: The
        LAST_NAME: Octocat
      run: |
        echo $MY_VAR $FIRST_NAME $MIDDLE_NAME $LAST_NAME.
```

### `jobs.<job_id>.steps.id`

步骤的唯一标识符。您可以使用`id`引用上下文中的步骤。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)”。

### `jobs.<job_id>.steps.if`

除非满足条件，否则您可以使用`if`条件语句来阻止步骤运行。您可以使用任何受支持的上下文和表达式来创建条件。

在`if`条件表达式中使用表达式时，您可以省略表达式语法（`${{ }}`），因为GitHub会自动将`if`条件表达式视为表达式。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)”。

**使用上下文的示例** 

仅当事件类型为`pull_request`并且事件动作为`unassigned`时，才运行此步骤。

```yaml
steps:
 - name: My first step
   if: ${{ github.event_name == 'pull_request' && github.event.action == 'unassigned' }}
   run: echo This event is a pull request that had an assignee removed.
```

**使用状态检查功能的示例**

`my backup step`仅在作业的上一步失败时运行。 有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#job-status-check-functions)”。

```yaml
steps:
  - name: My first step
    uses: monacorp/action-name@main
  - name: My backup step
    if: ${{ failure() }}
    uses: actions/heroku@master
```

### `jobs.<job_id>.steps.name`

您要在GitHub上显示的步骤的名称。

### `jobs.<job_id>.steps.uses`

选择要在您的工作步骤中运行的动作。动作是可重用的代码单元。您可以使用定义在与工作流相同的存储库，公共存储库或已[发布的Docker容器映像中](https://hub.docker.com/)的动作。

强烈建议您通过指定Git引用，SHA或Docker标记号来包含所使用动作的版本。如果您未指定版本，则在动作所有者发布更新时，它可能会中断您的工作流程或导致意外行为。

* 使用发布的动作版本的提交SHA是最稳定和安全的方式。
* 使用特定的主要动作版本，您可以在保持兼容性的同时获得关键的修补程序和安全补丁。它还可以确保您的工作流程仍然可以正常工作。
* 使用动作的默认分支可能很方便，但是如果有人发布具有重大更改的新主版本，则您的工作流程可能会中断。

某些动作要求您必须使用[`with`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepswith)关键字设置输入。查看动作的自述文件以确定所需的输入。

动作是JavaScript文件或Docker容器。如果您使用的动作是Docker容器，则必须在Linux环境中运行作业。有关更多详细信息，请参见[`runs-on`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on)。

**使用版本化动作的示例**

```yaml
steps:    
  # 引用特定提交
  - uses: actions/setup-node@74bc508
  # 引用释放的主要版本
  - uses: actions/setup-node@v1
  # 引用释放的次要版本
  - uses: actions/setup-node@v1.2
  # 引用分支
  - uses: actions/setup-node@main
```

**使用公共动作的示例**

`{owner}/{repo}@{ref}`

您可以在公共GitHub存储库中指定分支，引用或SHA。

```yaml
jobs:
  my_first_job:
    steps:
      - name: My first step
        # 使用公共存储库的默认分支
        uses: actions/heroku@master
      - name: My second step
        # 使用公共存储库的特定版本标记
        uses: actions/aws@v2.0.1
```

**在子目录中使用公共动作的示例**

`{owner}/{repo}/{path}@{ref}`

公用GitHub存储库中特定分支，引用或SHA的子目录。

```yaml
jobs:
  my_first_job:
    steps:
      - name: My first step
        uses: actions/aws/ec2@main
```

**在与工作流相同的存储库中使用动作的示例**

`./path/to/dir`

包含工作流存储库中动作的目录的路径。您必须先检出存储库，然后才能使用动作。

```yaml
jobs:
  my_first_job:
    steps:
      - name: Check out repository
        uses: actions/checkout@v2
      - name: Use local my-action
        uses: ./.github/actions/my-action
```

**使用Docker Hub动作的示例**

`docker://{image}:{tag}`

在[Docker Hub](https://hub.docker.com/)上发布的Docker映像。

```yaml
jobs:
  my_first_job:
    steps:
      - name: My first step
        uses: docker://alpine:3.8
```

**使用Docker公共注册表动作的示例**

`docker://{host}/{image}:{tag}`

公共注册表中的Docker映像。

```yaml
jobs:
  my_first_job:
    steps:
      - name: My first step
        uses: docker://gcr.io/cloud-builders/gradle
```

### `jobs.<job_id>.steps.run`

使用操作系统的shell程序运行命令行程序。如果不提供`name`，则步骤名称将默认为`run`命令中指定的文本。

默认情况下，命令使用非登录shell运行。您可以选择其他shell程序，并自定义用于运行命令的shell程序。有关更多信息，请参见“[使用特定的shell程序”](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#using-a-specific-shell)。

每个`run`关键字代表运行器环境中的新进程和shell。提供多行命令时，每行都在同一shell中运行。例如：

* 单行命令：

  ```yaml
  - name: Install Dependencies
    run: npm install
  ```

* 多行命令：

  ```yaml
  - name: Clean install dependencies and build
    run: |
      npm ci
      npm run build
  ```

  > 译者注：
  >
  > 上面的一个`name`条目下应该只有一个`run`，不使用`name`的多行命令[参考](https://github.com/actions/starter-workflows/blob/c59b62dee0eae1f9f368b7011cf05c2fc42cf084/ci/npm-publish.yml)
  >
  > ```yaml
  >     steps:
  >       - uses: actions/checkout@v2
  >       - uses: actions/setup-node@v1
  >         with:
  >           node-version: 12
  >           registry-url: https://registry.npmjs.org/
  >       - run: npm ci
  >       - run: npm publish
  > ```

使用`working-directory`关键字，您可以指定运行命令的工作目录。

```yaml
- name: Clean temp directory
  run: rm -rf *
  working-directory: ./temp
```

**使用特定的shell**

您可以使用`shell`关键字在运行者的操作系统中覆盖默认的Shell设置。您可以使用内置`shell`关键字，也可以定义一组自定义的shell程序选项。

| 支持平台      | `shell` 参数 | 描述                                                         | 命令在内部运行                                   |
| ------------- | ------------ | ------------------------------------------------------------ | ------------------------------------------------ |
| 所有          | `bash`       | 非Windows平台上的默认shell，后备版本为`sh`。在Windows上指定bash shell时，将使用Windows的Git随附的bash shell。 | `bash --noprofile --norc -eo pipefail {0}`       |
| 所有          | `pwsh`       | PowerShell核心。GitHub将扩展名`.ps1`附加到您的脚本名称。     | `pwsh -command ". '{0}'"`                        |
| 所有          | `python`     | 执行python命令。                                             | `python {0}`                                     |
| Linux / macOS | `sh`         | 如果未提供shell程序且`bash`在路径中未找到shell程序，则非Windows平台的回退行为。 | `sh -e {0}`                                      |
| Windows       | `cmd`        | GitHub将扩展名`.cmd`附加到您的脚本名称中，并替代`{0}`。      | `%ComSpec% /D /E:ON /V:OFF /S /C "CALL "{0}""`。 |
| Windows       | `powershell` | 这是Windows上使用的默认shell程序。桌面PowerShell。GitHub将扩展名`.ps1`附加到您的脚本名称。 | `powershell -command ". '{0}'"`。                |

**使用bash运行脚本示例**

```yaml
steps:
  - name: Display the path
    run: echo $PATH
    shell: bash
```

**使用Windows的`cmd`运行脚本示例** 

```yaml
steps:
  - name: Display the path
    run: echo %PATH%
    shell: cmd
```

**使用PowerShell Core运行脚本示例**

```yaml
steps:
  - name: Display the path
    run: echo ${env:PATH}
    shell: pwsh
```

**运行python脚本示例**

```yaml
steps:
  - name: Display the path
    run: |
      import os
      print(os.environ['PATH'])
    shell: python
```

**定制shell**

您可以使用`command […options] {0} [..more_options]`将`shell`值设置为模板字符串。 GitHub将字符串的第一个空格分隔的单词解释为命令，并在`{0}`处插入临时脚本的文件名。

**退出代码和错误动作首选项**

对于内置的shell关键字，我们提供了由GitHub托管的运行器执行的以下默认值。运行shell程序脚本时，应使用这些准则。

* `bash`/ `sh`：
  * 使用`set -e o pipefail`快速失败行为：`bash`内置默认值`shell`。在非Windows平台上不提供选项时，它也是默认设置。
  * 您可以通过向shell选项提供模板字符串来选择不执行快速失败并完全控制。例如，`bash {0}`。
  * 类似sh的shell会以脚本中执行的最后一个命令的退出代码退出，这也是动作的默认行为。运行程序将基于此退出代码将步骤状态报告为失败/成功。
* `powershell`/`pwsh`
  * 尽可能采用快速失败行为。对于`pwsh`和`powershell`内置的shell，我们会预先考虑`$ErrorActionPreference = 'stop'`到脚本内容。
  * 我们将附加 `if ((Test-Path -LiteralPath variable:\LASTEXITCODE)) { exit $LASTEXITCODE }`  到powershell脚本中，以便动作状态反映脚本的最后退出代码。
  * 用户始终可以通过不使用内置shell程序而退出，并根据需要提供自定义shell程序选项，例如：`pwsh -File {0}`或`powershell -Command "& '{0}'"`。
* `cmd`
  * 除了编写脚本来检查每个错误代码并做出相应响应之外，似乎没有其他方法可以完全采用快速失败行为。由于默认情况下我们实际上无法提供该行为，因此您需要将此行为写入脚本。
  * `cmd.exe`将退出，并返回其执行的最后一个程序的错误级别，并将错误代码返回给运行程序。此行为在内部与先前的`sh`和`pwsh`默认行为一致，并且是默认`cmd.exe`，因此此行为保持不变。

### `jobs.<job_id>.steps.with`

动作定义的输入参数`map`。每个输入参数都是一个键/值对。输入参数设置为环境变量。变量带有前缀`INPUT_`并转换为大写。

**示例**

定义`hello_world`动作的三个输入参数（`first_name`，`middle_name`和`last_name`）。这些输入变量将作为`INPUT_FIRST_NAME`、`INPUT_MIDDLE_NAME`和`INPUT_LAST_NAME`环境变量被`hello-world`动作访问。

```yaml
jobs:
  my_first_job:
    steps:
      - name: My first step
        uses: actions/hello_world@main
        with:
          first_name: Mona
          middle_name: The
          last_name: Octocat      
```

### `jobs.<job_id>.steps.with.args`

一个`string`，定义了这个Docker容器的输入。容器启动时，GitHub将传递`args`给容器`ENTRYPOINT`。一个`array of strings`不受此参数的支持。

**示例**

```yaml
steps:
  - name: Explain why this job ran
    uses: monacorp/action-name@main
    with:
      entrypoint: /bin/echo
      args: The ${{ github.event_name }} event triggered this step.
```

在`Dockerfile`中用`args`代替`CMD`指令。如果您在`Dockerfile`中使用`CMD`，请使用按偏好排序的准则：

1. 在动作的自述文件中记录所需的参数，并在`CMD`指令中将其省略。
2. 使用允许在不指定任何`args`的情况下使用动作的默认值。
3. 如果该动作公开了一个`--help`标志或类似的标志，请使用该标志作为默认值以使您的动作具有自记录功能。 

### `jobs.<job_id>.steps.with.entrypoint`

覆盖`Dockerfile`中的Docker `ENTRYPOINT`，或者在尚未指定的情况下设置。 与具有shell和exec形式的Docker `ENTRYPOINT`指令不同，`entrypoint`关键字仅接受定义要运行的可执行文件的单个字符串。

**示例** 

```yaml
steps:
  - name: Run a custom command
    uses: monacorp/action-name@main
    with:
      entrypoint: /a/different/executable
```

该`entrypoint`关键字旨在与Docker容器动作一起使用，但是您也可以将其与未定义任何输入的JavaScript动作一起使用。

### `jobs.<job_id>.steps.env`

设置要在运行器环境中使用的步骤的环境变量。 您还可以为整个工作流程或作业设置环境变量。有关更多信息，请参见[`env`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#env)和[`jobs.<job_id>.env`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idenv)。

当使用同一个名称定义了多个环境变量时，GitHub将使用最具体的环境变量。例如，在执行步骤时，在步骤中定义的环境变量将覆盖具有相同名称的作业和工作流变量。在作业执行时，为作业定义的变量将覆盖具有相同名称的工作流程变量。

公共动作可以在README文件中指定预期的环境变量。如果要在环境变量中设置机密，则必须使用`secrets`上下文设置机密。有关更多信息，请参阅“[使用环境变量](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/using-environment-variables)”和“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)”。

**示例** 

```yaml
steps:
  - name: My first action
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      FIRST_NAME: Mona
      LAST_NAME: Octocat
```

### `jobs.<job_id>.steps.continue-on-error`

当步骤失败时，防止作业失败。设置为`true`允许此步骤失败时通过作业。

### `jobs.<job_id>.steps.timeout-minutes`

终止进程之前运行步骤的最大分钟数。

## `jobs.<job_id>.timeout-minutes`

在GitHub自动取消作业之前允许作业运行的最大分钟数。默认值：360

## `jobs.<job_id>.strategy`

策略为您的作业创建一个构建矩阵。可以定义运行每个作业的环境的不同变体。

### `jobs.<job_id>.strategy.matrix`

您可以定义不同作业配置的矩阵。矩阵允许您通过在单个作业定义中执行变量替换来创建多个作业。例如，您可以使用矩阵来为一种以上编程语言，操作系统或工具的受支持版本创建作业。矩阵会重用作业的配置，并为您配置的每个矩阵创建一个作业。

作业矩阵每个工作流程运行最多可生成256个作业。此限制也适用于自托管运行者。

您在`matrix`中定义的每个选项都有一个键和值。您定义的键将成为`matrix`上下文中的属性，您可以在工作流文件的其他区域中引用该属性。例如，如果定义包含操作系统数组的键`os`，则可以使用该`matrix.os`属性作为`runs-on`关键字的值来为每个操作系统创建作业。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)”。

您定义`matrix`的顺序很重要。您定义的第一个选项将是工作流中运行的第一个作业。

**使用不止一个版本的Node.js运行的示例**

您可以通过为配置选项提供数组来指定矩阵。例如，如果运行程序支持Node.js版本6、8和10，则可以在中指定这些版本的数组`matrix`。

本示例通过将`node`键设置为三个Node.js版本的数组来创建三个作业的矩阵。要使用矩阵，该示例将`matrix.node`上下文属性设置为`setup-node`动作的input参数`node-version`的值。结果，将运行三个作业，每个作业使用不同的Node.js版本。

```yaml
strategy:
  matrix:
    node: [6, 8, 10]
steps:
  # 配置GitHub托管运行程序上使用的node版本
  - uses: actions/setup-node@v1
    with:
      # Node.js 版本配置
      node-version: ${{ matrix.node }}
```

当使用GitHub托管的运行程序`setup-node`时，建议采取此动作来配置Node.js版本。有关更多信息，请参见[`setup-node`](https://github.com/actions/setup-node)动作。

**在多个操作系统上运行的示例**

您可以创建一个矩阵，以在多个运行程序操作系统上运行工作流。您还可以指定多个矩阵配置。本示例创建一个包含6个作业的矩阵：

* `os`数组中指定了2个操作系统
* `node`数组中指定了3个Node.js版本

定义操作系统矩阵时，必须将`runs-on`的值设置为已定义的`matrix.os`上下文属性。

```yaml
runs-on: ${{ matrix.os }}
strategy:
  matrix:
    os: [ubuntu-16.04, ubuntu-18.04]
    node: [6, 8, 10]
steps:
  - uses: actions/setup-node@v1
    with:
      node-version: ${{ matrix.node }}
```

要查找GitHub托管的运行程序的受支持配置选项，请参阅“ [GitHub托管的运行程序的虚拟环境](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/virtual-environments-for-github-hosted-runners)”。

**将附加值包含到组合中的示例**

您可以将其他配置选项添加到已经存在的构建矩阵作业中。例如，如果要在运行使用`windows-latest`的作业和`node`的版本4的作业时使用`npm`的特定版本，则可以使用`include`指定该附加选项。

```yaml
runs-on: ${{ matrix.os }}
strategy:
  matrix:
    os: [macos-latest, windows-latest, ubuntu-18.04]
    node: [4, 6, 8, 10]
    include:
      # 包含一个新的npm变量，其值为2，用于匹配操作系统和版本的矩阵分支
      - os: windows-latest
        node: 4
        npm: 2
```

**包含新组合的示例**

您可以用`include`来将新作业添加到构建矩阵。任何不匹配的包含配置都将添加到矩阵中。例如，如果您要使用`node`版本12在多个操作系统上构建，但是想要在Ubuntu上使用node版本13进行一项额外的实验性工作，则可以使用`include`来指定该额外工作。

```yaml
runs-on: ${{ matrix.os }}
strategy:
  matrix:
    node: [12]
    os: [macos-latest, windows-latest, ubuntu-18.04]
    include:
      - node: 13
        os: ubuntu-18.04
        experimental: true
```

**从矩阵中排除配置的示例**

您可以使用`exclude`选项删除在构建矩阵中定义的特定配置。使用`exclude`删除由构建矩阵定义的作业。作业数量是您提供的阵列中包含的操作系统数量（`os`）的乘积减去任何减法（`exclude`）的乘积。
```yaml
runs-on: ${{ matrix.os }}
strategy:
  matrix:
    os: [macos-latest, windows-latest, ubuntu-18.04]
    node: [4, 6, 8, 10]
    exclude:
      # excludes node 4 on macOS
      - os: macos-latest
        node: 4
```

> **注意：**所有`include`组合均在`exclude`之后处理。这使您可以用`include`添加回先前被排除的组合。

## `jobs.<job_id>.strategy.fail-fast`

设置`true`为时，如果任何`matrix`作业失败，GitHub会取消所有正在进行的作业。默认：`true`

## `jobs.<job_id>.strategy.max-parallel`

使用`matrix`作业策略时可以同时运行的最大作业数。默认情况下，GitHub将根据GitHub托管的虚拟机上可用的运行程序最大化并行运行的作业数。

```yaml
strategy:
  max-parallel: 2
```

## `jobs.<job_id>.continue-on-error`

防止作业失败时工作流运行失败。设置为`true`允许此作业失败时通过工作流程。

**防止特定失败的矩阵作业使工作流运行失败的示例**

您可以允许作业矩阵中的特定作业失败而不会导致工作流运行失败。例如，如果您只想将`node`设置为`13`的实验性作业失败而不使工作流运行失败。

```yaml
runs-on: ${{ matrix.os }}
continue-on-error: ${{ matrix.experimental }}
strategy:
  fail-fast: false
  matrix:
    node: [11, 12]
    os: [macos-latest, ubuntu-18.04]
    experimental: [false]
    include:
      - node: 13
        os: ubuntu-18.04
        experimental: true
```

## `jobs.<job_id>.container`

一个容器，用于运行作业中尚未指定容器的任何步骤。如果您具有同时使用脚本动作和容器动作的步骤，则这些容器动作将作为同一个容器在同一个网络上使用相同的卷安装来运行。

如果未设置 `container`，则所有步骤将直接在`runs-on`指定的主机上运行，除非某个步骤引用了配置为在容器中运行的动作。

**示例**

```yaml
jobs:
  my_job:
    container:
      image: node:10.16-jessie
      env:
        NODE_ENV: development
      ports:
        - 80
      volumes:
        - my_docker_volume:/volume_mount
      options: --cpus 1
```

仅指定容器镜像时，可以省略`image`关键字。

```yaml
jobs:
  my_job:
    container: node:10.16-jessie
```

### `jobs.<job_id>.container.image`

用作运行动作的容器的Docker镜像。该值可以是Docker Hub镜像名称或注册表名称。

### `jobs.<job_id>.container.credentials`

如果映像的容器注册表需要身份验证才能拉取映像，则可以使用`credentials`来设置`username`和`password`的`map`。凭据与您将提供给[`docker login`](https://docs.docker.com/engine/reference/commandline/login/)命令的值相同。

**示例**

```yaml
container:
  image: ghcr.io/owner/image
  credentials:
     username: ${{ github.actor }}
     password: ${{ secrets.ghcr_token }}
```

### `jobs.<job_id>.container.env`

在容器中设置环境变量`map` 。

### `jobs.<job_id>.container.ports`

设置要在容器上公开的端口`array`。

### `jobs.<job_id>.container.volumes`

设置容器使用的卷`array`。您可以使用卷在服务或作业中的其他步骤之间共享数据。您可以指定命名的Docker卷，匿名Docker卷或在主机上绑定安装。

要指定卷，请指定源路径和目标路径：

`<source>:<destinationPath>`。

`<source>`是一个卷名或主机上的绝对路径，并且`<destinationPath>`是在容器中的绝对路径。

**示例**

```yaml
volumes:
  - my_docker_volume:/volume_mount
  - /data/my_data
  - /source/directory:/destination/directory
```

### `jobs.<job_id>.container.options`

其他Docker容器资源选项。有关选项的列表，请参见“[`docker create`选项”](https://docs.docker.com/engine/reference/commandline/create/#options)。

## `jobs.<job_id>.services`

> **注意：**如果您的工作流使用Docker容器动作或服务容器，则必须使用Linux运行器：
>
> * 如果您使用的是GitHub托管的运行程序，则必须使用Ubuntu运行程序。
> * 如果您使用的是自托管运行程序，则必须使用Linux计算机作为运行程序，并且必须安装Docker。

用于为工作流中的作业托管服务容器。服务容器对于创建数据库或缓存服务（例如Redis）很有用。运行程序自动创建Docker网络并管理服务容器的生命周期。

如果您将作业配置为在容器中运行，或者您的步骤使用容器动作，则无需映射端口即可访问服务或动作。Docker自动在同一Docker用户定义的网桥网络上公开容器之间的所有端口。您可以通过服务容器的主机名直接引用它。主机名会自动映射到您在工作流程中为服务配置的标签名。

如果您将作业配置为直接在运行器计算机上运行，并且您的步骤不使用容器动作，则必须将所有必需的Docker服务容器端口映射到Docker主机（运行器计算机）。您可以使用localhost和映射的端口访问服务容器。

有关网络服务容器之间差异的更多信息，请参阅“[关于服务容器”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/about-service-containers)。

**使用本地主机的示例**

本示例创建两个服务：nginx和redis。当您指定Docker主机端口而不是容器端口时，容器端口会随机分配给空闲端口。GitHub在`${{job.services.<service_name>.ports}}`上下文中设置分配的容器端口。在此示例中，您可以使用`${{ job.services.nginx.ports['8080'] }}`和`${{ job.services.redis.ports['6379'] }}`上下文访问服务容器端口。

```yaml
services:
  nginx:
    image: nginx
    # 将Docker主机上的端口8080映射到nginx容器上的端口80
    ports:
      - 8080:80
  redis:
    image: redis
    # 将Docker主机上的TCP端口6379映射到Redis容器上的随机空闲端口
    ports:
      - 6379/tcp
```

### `jobs.<job_id>.services.<service_id>.image`

要用作运行动作的服务容器的Docker映像。 该值可以是Docker Hub映像名称或注册表名称。

### `jobs.<job_id>.services.<service_id>.credentials`

如果镜像的容器注册表需要验证才能拉取镜像，你可以使用`credentials`设置`username`和`password`的`map`。凭据与您将提供给[`docker login`](https://docs.docker.com/engine/reference/commandline/login/)命令的值相同。

**示例**

```yaml
services:
  myservice1: 
    image: ghcr.io/owner/myservice1
    credentials:
      username: ${{ github.actor }}
      password: ${{ secrets.ghcr_token }}
  myservice2:
    image: dockerhub_org/myservice2
    credentials:
      username: ${{ secrets.DOCKER_USER }}
      password: ${{ secrets.DOCKER_PASSWORD }}
```

### `jobs.<job_id>.services.<service_id>.env`

在服务容器中设置环境变量`map` 。

### `jobs.<job_id>.services.<service_id>.ports`

设置要在服务容器上公开的端口`array`。

### `jobs.<job_id>.services.<service_id>.volumes`

设置要使用的服务容器的卷`array`。您可以使用卷在服务或作业中的其他步骤之间共享数据。您可以指定命名的Docker卷，匿名Docker卷或在主机上绑定安装。

要指定卷，请指定源路径和目标路径：

`<source>:<destinationPath>`。

`<source>`是一个卷名或主机上的绝对路径，并且`<destinationPath>`是在容器中的绝对路径。

**示例** 

```yaml
volumes:
  - my_docker_volume:/volume_mount
  - /data/my_data
  - /source/directory:/destination/directory
```

### `jobs.<job_id>.services.<service_id>.options`

其他Docker容器资源选项。有关选项的列表，请参见“[`docker create`选项”](https://docs.docker.com/engine/reference/commandline/create/#options)。

## 过滤模式备忘单

您可以在路径，分支和标签过滤器中使用特殊字符。

* `*`：匹配零个或多个字符，但不匹配该`/`字符。例如，`Octo*`匹配`Octocat`。
* `**`：匹配零个或多个任何字符。
* `?`：匹配零个或一个单个字符。例如，`Octoc?t`匹配 `Octocat`。
* `+`：匹配一个或多个前面的字符。
* `[]`：匹配括号中所列或范围内的一个字符。范围只能包括`a-z`，`A-Z`，和`0-9`。例如，范围`[0-9a-f]`匹配任何数字或小写字母。例如，`[CB]at`匹配项`Cat`或`Bat`和`[1-2]00`匹配项`100`和`200`。
* `!`：在模式开始时使其否定先前的正模式。如果不是第一个字符，它没有特殊含义。

字符`*`，`[`和`!`是YAML中的特殊字符。如果以`*`，`[`或`!`开头一个模式，则必须将该模式用引号引起来。

```yaml
# Valid
- '**/README.md'

# 无效-创建阻止工作流运行的分析错误。
- **/README.md
```

有关分支，标记和路径过滤器语法的更多信息，请参见[`on.<push|pull_request>.<branches|tags>`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestbranchestags)“和” [`on.<push|pull_request>.paths`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestpaths)。

### 匹配分支和标签的模式

| 模式                                  | 描述                                                         | 匹配示例                                                     |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `feature/*`                           | 该`*`通配符匹配任何字符，但不匹配斜线（`/`）。               | `feature/my-branch` <br>`feature/your-branch`                |
| `feature/**`                          | 该`**`通配符匹配任何字符，包括斜杠（`/`分支和标签名称）。    | `feature/beta-a/my-branch` <br> `feature/your-branch` <br>`feature/mona/the/octocat` |
| `main`<br> `releases/mona-the-octcat` | 与分支名称或标记名称的确切名称匹配。                         | `main` <br>`releases/mona-the-octocat`                       |
| `'*'`                                 | 匹配所有不包含斜杠（`/`）的分支和标记名称。该`*`字符是YAML中的特殊字符。当以开头的模式时`*`，必须使用引号。 | `main` <br>`releases`                                        |
| `'**'`                                | 匹配所有分支和标记名称。不使用`branches`或`tags`过滤器时，这是默认行为。 | `all/the/branches` `every/tag`                               |
| `'*feature'`                          | 该`*`字符是YAML中的特殊字符。当以开头的模式时`*`，必须使用引号。 | `mona-feature` <br> `feature` <br>`ver-10-feature`           |
| `v2*`                                 | 匹配以开头的分支和标记名称`v2`。                             | `v2` <br> `v2.0` <br>`v2.9`                                  |
| `v[12].[0-9]+.[0-9]+`                 | 将所有语义版本标记与主版本1或2匹配                           | `v1.10.1` <br>`v2.0.0`                                       |

### 匹配文件路径的模式

路径模式必须与整个路径匹配，并且必须从存储库的根开始。

| 模式                                  | 比赛描述                                                     | 匹配示例                                                     |
| ------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `'*'`                                 | 该`*`通配符匹配任何字符，但不匹配斜线（`/`）。该`*`字符是YAML中的特殊字符。当以开头的模式时`*`，必须使用引号。 | `README.md`<br>`server.rb`                                   |
| `'*.jsx?'`                            | 该`?`字符与零或前一个字符匹配。                              | `page.js` <br>`page.jsx`                                     |
| `'**'`                                | 该`**`通配符任何字符，包括斜线匹配（`/`）。当您不使用`path`过滤器时，这是默认行为。 | `all/the/files.md`                                           |
| `'*.js'`                              | 该`*`通配符匹配任何字符，但不匹配斜线（`/`）。匹配存储库根目录中的所有`.js`文件。 | `app.js` <br>`index.js`                                      |
| `'**.js'`                             | 匹配存储库中的所有`.js`文件。                                | `index.js` <br> `js/index.js` <br>`src/js/app.js`            |
| `docs/*`                              | 存储库根目录中`docs`目录下的所有文件。                       | `docs/README.md` `docs/file.txt`                             |
| `docs/**`                             | 存储库根目录中`/docs`目录中的所有文件。                      | `docs/README.md` `docs/mona/octocat.txt`                     |
| `docs/**/*.md`                        | `docs`目录中任何位置带有`.md`后缀的文件。                    | `docs/README.md`  `docs/mona/hello-world.md` `docs/a/markdown/file.md` |
| `'**/docs/**'`                        | 存储库中任何位置的`docs`目录中的任何文件。                   | `/docs/hello.md`  `dir/docs/my-file.txt` `space/docs/plan/space.doc` |
| `'**/README.md'`                      | 存储库中任何位置的README.md文件。                            | `README.md` `js/README.md`                                   |
| `'**/*src/**'`                        | 存储库中任何地方都带有`src`后缀的文件夹中的任何文件。        | `a/src/app.js` <br>`my-src/code/js/app.js`                   |
| `'**/*-post.md'`                      | 在存储库中任何位置带有`-post.md`后缀的文件。                 | `my-post.md` <br>`path/their-post.md`                        |
| `'**/migrate-*.sql'`                  | 在存储库中任何位置带有前缀`migrate-`和后缀`.sql`的文件。     | `migrate-10909.sql`  `db/migrate-v1.0.sql` `db/sept/migrate-v1.sql` |
| `*.md` <br>`!README.md`               | 在模式前面使用感叹号（`!`）将其取反。当文件与某个模式匹配并且还与文件中稍后定义的否定模式匹配时，该文件将不包括在内。 | `hello.md` <br>*不匹配* <br> `README.md` <br>`docs/hello.md` |
| `*.md` <br> `!README.md`<br>`README*` | 顺序检查模式。否定先前模式的模式将重新包含文件路径。         | `hello.md` <br> `README.md` <br>`README.doc`                 |



## 参考阅读


