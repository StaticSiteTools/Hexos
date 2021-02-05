---
title: 触发工作流程的事件
date: 2020-11-07  8:04:04
updated: 2020-11-07  8:04:04
mathjax: false
categories: 
tags:
typora-root-url: events-that-trigger-workflows
typora-copy-images-to: events-that-trigger-workflows
top: 1
comments: false
---


# 触发工作流程的事件

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)

> 您可以将工作流配置为在GitHub上发生特定活动，在预定时间或GitHub外部事件发生时运行。 



## 配置工作流事件

您可以使用工作流程语法`on`将工作流程配置为针对一个或多个事件运行。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#on)”。

### 使用单个事件的示例

```yaml
# 当代码被推送到存储库中的任何分支时触发
on: push
```

### **使用事件列表的示例**

```yaml
# 触发推或拉请求事件的工作流
on: [push, pull_request]
```

### **使用具有活动类型或配置的多个事件的示例**

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
  # 也会触发页面生成，以及释放创建的事件
  page_build:
  release:
    types: # 此配置不影响上面的页面生成事件
      - created
```

> **注意：**您无法使用`GITHUB_TOKEN`触发新的工作流程运行。有关更多信息，请参阅“[使用个人访问令牌触发新的工作流](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows#triggering-new-workflows-using-a-personal-access-token)”。

发生以下步骤来触发工作流运行：

1. 您的存储库上发生了一个事件，并且所产生的事件具有关联的提交SHA和Git引用。

2. 在关联的提交SHA或Git引用中搜索存储库中的`.github/workflows`目录以查找工作流文件。必须在提交SHA或Git引用中存在工作流文件。

   例如，如果事件发生在特定的存储库分支上，则工作流文件必须存在于该分支上的存储库中。

3. 检查提交SHA和Git引用的工作流程文件，并为所有`on:`值与触发事件匹配的工作流程触发新的工作流程运行。

   工作流在存储库的代码上以触发事件的相同提交SHA和Git 引用运行。当工作流运行时，GitHub在运行器环境中设置`GITHUB_SHA`（提交SHA）和`GITHUB_REF`（Git 引用）环境变量。有关更多信息，请参见“[使用环境变量”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/using-environment-variables)。

## 计划事件

该`schedule`事件使您可以在计划的时间触发工作流。

### `schedule`

| Webhook事件负载 | 活动类型 | `GITHUB_SHA`               | `GITHUB_REF` |
| --------------- | -------- | -------------------------- | ------------ |
| n/a             | n/a      | 在默认分支上的最后一次提交 | 默认分支     |

您可以使用[POSIX cron语法](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html#tag_20_25_07)安排工作流在特定的UTC时间运行。计划的工作流在默认或基本分支上的最新提交上运行。您可以运行计划的工作流程的最短间隔是每5分钟一次。

此示例每15分钟触发一次工作流：

```yaml
on:
  schedule:
    # * 是YAML中的一个特殊字符，因此必须引号此字符串
    - cron:  '*/15 * * * *'
```

Cron语法有五个用空格隔开的字段，每个字段代表一个时间单位。

```
┌───────────── 分钟 (0 - 59)
│ ┌───────────── 时 (0 - 23)
│ │ ┌───────────── 天 (1 - 31)
│ │ │ ┌───────────── 月 (1 - 12 or JAN-DEC)
│ │ │ │ ┌───────────── 周 (0 - 6 or SUN-SAT)
│ │ │ │ │                                   
│ │ │ │ │
│ │ │ │ │
* * * * *
```

您可以在以下五个字段中的任何一个中使用这些运算符：

| 操作员 | 描述         | 例                                                           |
| ------ | ------------ | ------------------------------------------------------------ |
| `*`    | 任何值       | `* * * * *` 每天每一分钟运行。                               |
| `，`   | 值列表分隔符 | `2,10 4,5 * * *` 在每天的第4和5小时的第2分钟和第10分钟运行。 |
| `-`    | 取值范围     | `0 4-6 * * *` 在第4、5和6小时的第0分钟运行。                 |
| `/`    | 步长值       | `20/15 * * * *` 从20分钟到59（每20、35和50分钟）开始，每15分钟运行一次。 |

> **注：** GitHub的操作不支持非标准的语法`@yearly`，`@monthly`，`@weekly`，`@daily`，`@hourly`，和`@reboot`。

您可以使用[crontab guru](https://crontab.guru/)来帮助生成您的cron语法并确认它将在什么时间运行。为了帮助您入门，还有[crontab guru示例](https://crontab.guru/examples.html)列表。



## 手动事件

您可以手动触发工作流程运行。要触发存储库中的特定工作流程，请使用`workflow_dispatch`事件。要触发存储库中的多个工作流程并创建自定义事件和事件类型，请使用`repository_dispatch`事件。

### `workflow_dispatch`

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`                 | `GITHUB_REF`   |
| ------------------------------------------------------------ | -------- | ---------------------------- | -------------- |
| [workflow_dispatch](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#workflow_dispatch) | 不适用   | `GITHUB_REF`分支上的最后提交 | 收到调度的分支 |

您可以直接在工作流中为事件配置自定义的输入属性、默认输入值和必需的输入。 当工作流运行时，您可以在`github.event.inputs`上下文中访问输入值。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#github-context)”。

您可以使用GitHub API和GitHub手动触发工作流程运行。有关更多信息，请参阅“[手动运行工作流程”](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/manually-running-a-workflow)。

在GitHub上触发事件时，您可以直接在GitHub上提供`ref`和`inputs`。有关更多信息，请参见“[将输入和输出与动作配合使用](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions#using-inputs-and-outputs-with-an-action)”。

要使用REST API触发自定义的`workflow_dispatch`  webhook事件，您必须向GitHub API端点发送`POST`请求，并提供`ref`和以及所有必需的`inputs`。有关更多信息，请参见“[创建工作流调度事件](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions/#create-a-workflow-dispatch-event)” REST API端点。

**工作流程配置示例**

本示例定义`name`和`home`输入，并使用`github.event.inputs.name`和`github.event.inputs.home`上下文打印它们。如果`name`未提供，则会打印默认值'Mona the Octocat'。

```yaml
name: Manually triggered workflow
on:
  workflow_dispatch:
    inputs:
      name:
        description: 'Person to greet'
        required: true
        default: 'Mona the Octocat'
      home:
        description: 'location'
        required: false

jobs:
  say_hello:
    runs-on: ubuntu-latest
    steps:
    - run: |
        echo "Hello ${{ github.event.inputs.name }}!"
        echo "- in ${{ github.event.inputs.home }}!"
```

### `repository_dispatch`

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`                 | `GITHUB_REF`   |
| ------------------------------------------------------------ | -------- | ---------------------------- | -------------- |
| [repository_dispatch](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#repository_dispatch) | 不适用   | `GITHUB_REF`分支上的最后提交 | 收到调度的分支 |

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

当您想为在GitHub之外发生的活动触发工作流时，可以使用GitHub API来触发一个名为[`repository_dispatch`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#repository_dispatch)的webhook事件。有关更多信息，请参阅“[创建存储库调度事件”](https://docs.github.com/en/free-pro-team@latest/v3/repos/#create-a-repository-dispatch-event)。

要触发自定义`repository_dispatch`  webhook事件，您必须向GitHub API端点发送`POST`请求，并提供描述活动类型的`event_type`名称。要触发工作流运行，还必须配置工作流以使用该`repository_dispatch`事件。

**示例**

默认情况下，所有这些`event_types`都会触发工作流程运行。当在`repository_dispatch` webhook有效载荷中发送一个特定的`event_type`值时，可以限制工作流程运行。在创建存储库调度事件时，可以定义在`repository_dispatch`负载中发送的事件类型。

```yaml
on:
  repository_dispatch:
    types: [opened, deleted]
```

## Webhook事件

您可以将工作流配置为在GitHub上创建webhook事件时运行。某些事件具有多种触发事件的活动类型。如果有多个活动类型触发事件，则可以指定哪些活动类型将触发工作流程运行。有关更多信息，请参见“ [Webhooks”](https://docs.github.com/en/free-pro-team@latest/webhooks)。

### `check_run`

`check_run`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[Check runs](https://docs.github.com/en/free-pro-team@latest/v3/checks/runs) “。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型                                                     | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | ------------ |
| [`check_run`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#check_run) | `created` <br /> `rerequested`<br /> `completed` <br />`requested_action` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当检查运行为`rerequested`或`requested_action`时，您可以运行工作流程。

```yaml
on:
  check_run:
    types: [rerequested, requested_action]
```

### `check_suite`

`check_suite`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅[Check suites](https://docs.github.com/en/free-pro-team@latest/v3/checks/suites). 

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。
>
> **注意：**为防止递归工作流，如果检查套件是由GitHub Actions创建的，则此事件不会触发工作流。

| Webhook事件负载                                              | 活动类型                                         | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | ------------------------------------------------ | -------------------------- | ------------ |
| [`check_suite`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#check_suite) | `completed` <br />`requested`<br />`rerequested` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当检查套件为`rerequested`或`completed`时，您可以运行工作流程。

```yaml
on:
  check_suite:
    types: [rerequested, completed]
```

### `create`

每当有人创建分支或标签时触发`create`事件，运行您的工作流。有关REST API的信息，请参阅“[创建参考 ](https://docs.github.com/en/free-pro-team@latest/v3/git/refs/#create-a-reference)“。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`                   | `GITHUB_REF`       |
| ------------------------------------------------------------ | -------- | ------------------------------ | ------------------ |
| [`create`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#create) | 不适用   | 在创建的分支或标签上的最后提交 | 已创建的分支或标记 |

例如，您可以在`create`事件发生时运行工作流程。

```yaml
on:
  create
```

### `delete`

每当有人删除分支或标签时触发`delete`事件，运行您的工作流。有关REST API的信息，请参阅“[删除参考”](https://docs.github.com/en/free-pro-team@latest/v3/git/refs/#delete-a-reference)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | -------- | -------------------------- | ------------ |
| [`delete`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#delete) | 不适用   | 在默认分支上的最后一次提交 | 默认分支     |

例如，您可以在`delete`事件发生时运行工作流程。

```yaml
on:
  delete
```

### `deployment`

每当有人创建部署（触发`deployment`事件）时就运行您的工作流。使用提交SHA创建的部署可能没有Git引用。有关REST API的信息，请参阅“[部署”](https://docs.github.com/en/free-pro-team@latest/rest/reference/repos#deployments)。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA` | `GITHUB_REF`                         |
| ------------------------------------------------------------ | -------- | ------------ | ------------------------------------ |
| [`deployment`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#deployment) | 不适用   | 提交部署     | 要部署的分支或标签（如果提交则为空） |

例如，您可以在`deployment`事件发生时运行工作流程。

```yaml
on:
  deployment
```

### `deployment_status`

只要第三方提供部署状态（触发`deployment_status`事件），便可以运行您的工作流。使用提交SHA创建的部署可能没有Git引用。有关REST API的信息，请参阅“[创建部署状态”](https://docs.github.com/en/free-pro-team@latest/rest/reference/repos#create-a-deployment-status)。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA` | `GITHUB_REF`                         |
| ------------------------------------------------------------ | -------- | ------------ | ------------------------------------ |
| [`deployment_status`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#deployment_status) | 不适用   | 提交部署     | 要部署的分支或标签（如果提交则为空） |

例如，您可以在`deployment_status`事件发生时运行工作流程。

```yaml
on:
  deployment_status
```

### `fork`

有人分叉存储库（触发`fork`事件）时，随时运行您的工作流。有关REST API的信息，请参阅“[创建派生”](https://docs.github.com/en/free-pro-team@latest/v3/repos/forks/#create-a-fork)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | -------- | -------------------------- | ------------ |
| [`fork`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#fork) | 不适用   | 在默认分支上的最后一次提交 | 默认分支     |

例如，您可以在`fork`事件发生时运行工作流程。

```yaml
on:
  fork
```

### `gollum`

当有人创建或更新Wiki页面时触发`gollum`事件运行您的工作流程。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | -------- | -------------------------- | ------------ |
| [`gollum`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#gollum) | 不适用   | 在默认分支上的最后一次提交 | 默认分支     |

例如，您可以在`gollum`事件发生时运行工作流程。

```yaml
on:
  gollum
```

### `issue_comment`

`issue_comment`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[问题评论”](https://docs.github.com/en/free-pro-team@latest/developers/webhooks-and-events/webhook-events-and-payloads#issue_comment)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型                                 | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | ---------------------------------------- | -------------------------- | ------------ |
| [`issue_comment`](https://docs.github.com/en/free-pro-team@latest/rest/reference/activity#issue_comment) | `created`<br /> `edited` <br />`deleted` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当问题注释为`created`或`deleted`时，您可以运行工作流程。

```yaml
on:
  issue_comment:
    types: [created, deleted]
```

### `issues`

`issues`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[问题”](https://docs.github.com/en/free-pro-team@latest/v3/issues)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型                                                     | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | ------------ |
| [`issues`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#issues) | `opened` <br /> `edited` <br /> `deleted` <br /> `transferred`<br /> `pinned` <br /> `unpinned` <br /> `closed` <br /> `reopened` <br /> `assigned` <br /> `unassigned` <br /> `labeled`<br /> `unlabeled`<br />`locked`<br />`unlocked`<br /> `milestoned`<br />`demilestoned` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，你可以当一个`opened`，`edited`或`milestoned`问题发生时运行工作流。

```yaml
on:
  issues:
    types: [opened, edited, milestoned]
```

### `label`

`label`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[标签”](https://docs.github.com/en/free-pro-team@latest/v3/issues/labels)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型                                | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | --------------------------------------- | -------------------------- | ------------ |
| [`label`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#label) | `created` <br />`edited`<br />`deleted` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，您可以在标签为`created`或`deleted`时运行工作流程。

```yaml
on:
  label:
    types: [created, deleted]
```

### `milestone`

`milestone`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[里程碑”](https://docs.github.com/en/free-pro-team@latest/v3/issues/milestones)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型                                                | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | ------------------------------------------------------- | -------------------------- | ------------ |
| [`milestone`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#milestone) | - `created` - `closed` - `opened` - `edited` -`deleted` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，您可以在里程碑达到`opened`或`deleted`时运行工作流程。

```yaml
on:
  milestone:
    types: [opened, deleted]
```

### `page_build`

每当有人推送到启用了GitHub Pages的分支（该`page_build`事件触发）时，便运行您的工作流。有关REST API的信息，请参见“[页面”](https://docs.github.com/en/free-pro-team@latest/rest/reference/repos#pages)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | -------- | -------------------------- | ------------ |
| [`page_build`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#page_build) | 不适用   | 在默认分支上的最后一次提交 | 不适用       |

例如，您可以在`page_build`事件发生时运行工作流程。

```yaml
on:
  page_build
```

### `project`

`project`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[项目”](https://docs.github.com/en/free-pro-team@latest/v3/projects)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型                                                     | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | ------------ |
| [`project`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#project) | `created` <br /> `updated` <br /> `closed` <br /> `reopened` <br /> `edited` <br />`deleted` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当项目为`created`或`deleted`时，您可以运行工作流。

```yaml
on:
  project:
    types: [created, deleted]
```

### `project_card`

`project_card`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[项目卡”](https://docs.github.com/en/free-pro-team@latest/v3/projects/cards)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型                                                     | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | ------------ |
| [`project_card`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#project_card) | `created` <br />`moved`<br />`converted`到一个问题 <br />`edited` <br />`deleted` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当项目卡为`opened`或`deleted`时，您可以运行工作流程。

```yaml
on:
  project_card:
    types: [opened, deleted]
```

### `project_column`

`project_column`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参见“[项目列”](https://docs.github.com/en/free-pro-team@latest/v3/projects/columns)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型                                                 | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | -------------------------------------------------------- | -------------------------- | ------------ |
| [`project_column`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#project_column) | `created` <br /> `updated`<br /> `moved` <br />`deleted` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当项目列为`created`或`deleted`时，您可以运行工作流。

```yaml
on:
  project_column:
    types: [created, deleted]
```

### `public`

每当有人将私有存储库公开（这会触发`public`事件）时运行您的工作流。有关REST API的信息，请参阅“[编辑存储库”](https://docs.github.com/en/free-pro-team@latest/v3/repos/#edit)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | -------- | -------------------------- | ------------ |
| [`public`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#public) | 不适用   | 在默认分支上的最后一次提交 | 默认分支     |

例如，您可以在`public`事件发生时运行工作流程。

```yaml
on:
  public
```

### `pull_request`

`pull_request`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[拉取请求”](https://docs.github.com/en/free-pro-team@latest/v3/pulls)。

> **注意：** 默认情况下，工作流仅在`pull_request`的活动类型为`opened`、`synchronize`或`reopened`时运行。要触发更多活动类型的工作流，请使用`types`关键字。


| Webhook事件负载                                              | 活动类型                                                     | `GITHUB_SHA`                     | `GITHUB_REF`                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- | --------------------------------------- |
| [`pull_request`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#pull_request) | `assigned` <br />`unassigned` <br /> `labeled` <br /> `unlabeled` <br />`opened`<br /> `edited` <br /> `closed` <br /> `reopened` <br /> `synchronize` <br /> `ready_for_review` <br /> `locked` <br />`unlocked` <br />`review_requested` <br />`review_request_removed` | `GITHUB_REF`分支上的最后合并提交 | PR 合并分支 `refs/pull/:prNumber/merge` |

您可以使用`types`关键字扩展或限制默认活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，您可以在pull请求已经运行工作流`assigned`，`opened`，`synchronize`，或`reopened`。

```yaml
on:
  pull_request:
    types: [assigned, opened, synchronize, reopened]
```

**派生存储库的拉取请求事件**

> **注意：**当您从派生的存储库中打开拉取请求时，工作流不会在私有基础存储库上运行。

当您创建从派生(forked)存储库到基础存储库的拉取请求时，GitHub将`pull_request`事件发送到基础存储库，并且在派生存储库上不会发生拉取请求事件。

默认情况下，工作流不在派生的存储库上运行。您必须在派生存储库的“**动作”**选项卡中启用GitHub动作。

派生存储库的`GITHUB_TOKEN`权限是只读的。有关更多信息，请参见“[使用GITHUB_TOKEN进行身份验证](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/authenticating-with-the-github_token)”。

### `pull_request_review`

`pull_request_review`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[拉取请求审查”](https://docs.github.com/en/free-pro-team@latest/v3/pulls/reviews)。

| Webhook事件负载                                              | 活动类型                                      | `GITHUB_SHA`                     | `GITHUB_REF`                           |
| ------------------------------------------------------------ | --------------------------------------------- | -------------------------------- | -------------------------------------- |
| [`pull_request_review`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#pull_request_review) | `submitted` <br /> `edited` <br />`dismissed` | `GITHUB_REF`分支上的最后合并提交 | PR合并分支 `refs/pull/:prNumber/merge` |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当拉取请求审核为`edited`或`dismissed`时，您可以运行工作流程。

```yaml
on:
  pull_request_review:
    types: [edited, dismissed]
```

**派生存储库的拉取请求事件**

> **注意：**当您从派生的存储库中打开拉取请求时，工作流不会在私有基础存储库上运行。

当您创建从派生存储库到基础存储库的拉取请求时，GitHub将`pull_request`事件发送到基础存储库，并且在派生存储库上不会发生拉取请求事件。

默认情况下，工作流不在派生的存储库上运行。您必须在派生存储库的“**动作”**选项卡中启用GitHub动作。

派生存储库的`GITHUB_TOKEN`权限是只读的。有关更多信息，请参见“[使用GITHUB_TOKEN进行身份验证](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/authenticating-with-the-github_token)”。

### `pull_request_review_comment`

在拉取请求的统一差异的评论被修改时 （触发`pull_request_review_comment`事件），便可以运行您的工作流程。多种活动类型触发此事件。有关REST API的信息，请参阅[审核评论](https://docs.github.com/en/free-pro-team@latest/v3/pulls/comments)。

| Webhook事件负载                                              | 活动类型                                 | `GITHUB_SHA`                     | `GITHUB_REF`                           |
| ------------------------------------------------------------ | ---------------------------------------- | -------------------------------- | -------------------------------------- |
| [`pull_request_review_comment`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#pull_request_review_comment) | `created` <br />`edited` <br />`deleted` | `GITHUB_REF`分支上的最后合并提交 | PR合并分支 `refs/pull/:prNumber/merge` |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当拉取请求审阅注释为`created`或`deleted`时，您可以运行工作流程。

```yaml
on:
  pull_request_review_comment:
    types: [created, deleted]
```

**派生存储库的拉取请求事件**

> **注意：**当您从派生的存储库中打开拉取请求时，工作流不会在私有基础存储库上运行。

当您创建从派生存储库到基础存储库的拉取请求时，GitHub将`pull_request`事件发送到基础存储库，并且在派生存储库上不会发生拉取请求事件。

默认情况下，工作流不在派生的存储库上运行。您必须在派生存储库的“**动作”**选项卡中启用GitHub动作。

派生存储库的`GITHUB_TOKEN`权限是只读的。有关更多信息，请参见“[使用GITHUB_TOKEN进行身份验证](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/authenticating-with-the-github_token)”。

### `pull_request_target`

此事件与`pull_request`相似，不同之处在于它在拉取请求的基本存储库的上下文中运行，而不是在合并提交中运行。这意味着您可以更安全地将机密信息提供给拉请求触发的工作流，因为仅运行在基础存储库的提交中定义的工作流。例如，此事件使您可以基于事件负载的内容，创建对拉取请求进行标记和注释的工作流。

| Webhook事件负载                                              | 活动类型                                                     | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- | ------------ |
| [`pull_request`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#pull_request) | `assigned` <br />`unassigned`<br />`labeled` <br /> `unlabeled` <br /> `opened` <br /> `edited` <br /> `closed` <br /> `reopened` <br /> `synchronize`<br /> `ready_for_review` <br /> `locked` <br />`unlocked`  <br />`review_requested` <br />`review_request_removed` | PR基础分支上的最后一次提交 | PR基础分支   |

默认情况下，当一个工作流仅运行在`pull_request_target`的活动类型为`opened`，`synchronize`或`reopened`。要触发更多活动类型的工作流，请使用`types`关键字。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，您可以在拉取请求为`assigned`，`opened`，`synchronize`，或`reopened`时运行工作流

```yaml
on: pull_request_target
    types: [assigned, opened, synchronize, reopened]
```

### `push`

> **注：**适用于GitHub上操作网络挂接有效载荷不包括`added`，`removed`和`modified`属性在`commit`对象。您可以使用REST API检索完整的提交对象。有关更多信息，请参见“[获得单次提交](https://docs.github.com/en/free-pro-team@latest/v3/repos/commits/#get-a-single-commit)”。

当有人推送到存储库分支（触发`push`事件）时运行您的工作流。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`                         | `GITHUB_REF` |
| ------------------------------------------------------------ | -------- | ------------------------------------ | ------------ |
| [`push`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#push) | 不适用   | 提交已推送，除非删除分支（默认分支） | 更新引用     |

例如，您可以在`push`事件发生时运行工作流程。

```yaml
on:
  push
```

### `registry_package`

只要包为`published`或`updated`，即可执行您的工作流程。有关更多信息，请参阅“[使用GitHub软件包管理软件包](https://docs.github.com/en/free-pro-team@latest/github/managing-packages-with-github-packages)”。

| Webhook事件负载                                              | 活动类型                    | `GITHUB_SHA`       | `GITHUB_REF`             |
| ------------------------------------------------------------ | --------------------------- | ------------------ | ------------------------ |
| [`registry_package`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#package) | `published` <br />`updated` | 提交已发布的软件包 | 已发布软件包的分支或标签 |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，您可以在`published`包软件后运行工作流程。

```yaml
on:
  registry_package:
    types: [published]
```

### `release`

> **注意：**  草稿版本不会触发`release`事件。

`release`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[发行版”](https://docs.github.com/en/free-pro-team@latest/v3/repos/releases)。

| Webhook事件负载                                              | 活动类型                                                     | `GITHUB_SHA`         | `GITHUB_REF` |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- | ------------ |
| [`release`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#release) | `published`  <br />`unpublished` <br /> `created`<br /> `edited`  <br /> `deleted` <br /> `prereleased`<br />`released` | 标记版本中的最后提交 | 发行标签     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，当发布版本为`published` 时，您可以运行工作流。

```yaml
on:
  release:
    types: [published]
```

### `status`

只要Git提交的状态发生变化（触发`status`事件），就可以运行您的工作流程。有关REST API的信息，请参阅[状态](https://docs.github.com/en/free-pro-team@latest/v3/repos/statuses)。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型 | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | -------- | -------------------------- | ------------ |
| [`status`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#status) | 不适用   | 在默认分支上的最后一次提交 | 不适用       |

例如，您可以在`status`事件发生时运行工作流程。

```yaml
on:
  status
```

### `watch`

`watch`事件发生时随时运行您的工作流。多种活动类型触发此事件。有关REST API的信息，请参阅“[watch ](https://docs.github.com/en/free-pro-team@latest/v3/activity/starring)”。

> **注意：**仅当工作流文件位于默认分支上时，此事件才会触发工作流运行。

| Webhook事件负载                                              | 活动类型  | `GITHUB_SHA`               | `GITHUB_REF` |
| ------------------------------------------------------------ | --------- | -------------------------- | ------------ |
| [`watch`](https://docs.github.com/en/free-pro-team@latest/webhooks/event-payloads/#watch) | `started` | 在默认分支上的最后一次提交 | 默认分支     |

默认情况下，所有活动类型都会触发工作流程运行。您可以使用`types`关键字将工作流程运行限制为特定的活动类型。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#onevent_nametypes)”。

例如，您可以在有人启动存储库时运行工作流，该存储库是`started`触发监视事件的活动类型。

```yaml
on:
  watch:
    types: [started]
```

### `workflow_run`

当请求或完成工作流程运行时，会发生此事件，并使您可以根据另一个工作流程的完成结果执行工作流程。例如，如果您的`pull_request`工作流生成了构建工件，那么您可以使用`workflow_run`创建一个新的工作流，该工作流用于分析结果并为原始的拉取请求添加注释。

由`workflow_run`事件启动的工作流能够访问机密并写入原始工作流使用的令牌。

如果您需要从该事件中过滤分支，则可以使用`branches`或`branches-ignore`。

在此示例中，工作流配置为在单独的“运行测试”工作流完成后运行。

```yaml
on:
  workflow_run:
    workflows: ["Run Tests"]
    branches: [main]
    types: 
      - completed
      - requested
```

## 使用个人访问令牌触发新的工作流程

当您使用存储库的`GITHUB_TOKEN`代表GitHub Actions应用程序执行任务时，由`GITHUB_TOKEN`触发的事件将不会创建新的工作流程运行。这样可以防止您意外创建递归工作流运行。例如，如果工作流运行使用存储库的`GITHUB_TOKEN`推送代码，即使存储库包含配置为在`push`事件发生时运行的工作流，新的工作流也不会运行。有关更多信息，请参见“[使用GITHUB_TOKEN进行身份验证](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/authenticating-with-the-github_token)”。

如果要从工作流程运行中触发工作流程，则可以使用个人访问令牌触发事件。您需要创建一个个人访问令牌并将其存储为秘密。为了最大程度地降低GitHub Actions的使用成本，请确保您不创建递归或意外的工作流运行。有关更多信息，请参见“[创建和存储加密的机密”](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)。





## 参考阅读


