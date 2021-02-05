---
title: GitHub Actions简介
date: 2020-10-31  0:38:13
updated: 2020-10-31  0:38:13
mathjax: false
categories: 
tags:
typora-root-url: introduction-to-github-actions
typora-copy-images-to: introduction-to-github-actions
top: 1
comments: false
---


# GitHub Actions简介

[原文](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)

> 了解GitHub Actions的核心概念和各个组件，并查看一个示例，向您展示如何向存储库添加自动化。 



## 总览

GitHub Actions可帮助您在软件开发生命周期内自动化任务。GitHub Actions是事件驱动的，这意味着您可以在发生指定事件后运行一系列命令。例如，每当有人为存储库创建拉取请求时，您可以自动运行执行软件测试脚本的命令。

该图演示了如何使用GitHub Actions自动运行软件测试脚本。事件自动触发包含**作业(Job)**的**工作流程(Workflow)**。然后，作业将使用**步骤(Steps)**来控制**动作(Action)**的运行顺序。这些动作是使您的软件测试自动化的命令。

![工作流程概述](/overview-actions-simple.png)





## GitHub Actions的组件

下面是多个协同运行作业的GitHub Actions组件的列表。您可以看到这些组件是如何相互作用的。

![组件和服务概述](/overview-actions-design.png)

### 工作流程(Workflows)

工作流程是您添加到存储库中的自动化过程。工作流由一个或多个作业组成，可以由事件安排或触发。该工作流程可用于在GitHub上构建，测试，打包，发布或部署项目。

### 事件(Events)

事件是触发工作流程的特定活动。例如，当有人将提交推送到存储库或创建问题或请求请求时，活动可以源自GitHub。您还可以使用存储库调度Webhook在发生外部事件时触发工作流。有关可用于触发工作流的事件的完整列表，请参阅《[触发工作流的事件》](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)。

### 作业(Jobs)

作业是在同一运行程序上执行的一组步骤。默认情况下，具有多个作业的工作流程将并行运行这些作业。您还可以配置工作流以按顺序运行作业。例如，一个工作流可以有两个顺序的作业来构建和测试代码，其中测试作业取决于构建作业的状态。如果构建作业失败，则测试作业将不会运行。

### 步骤(Steps)

步骤是可以运行命令的单个任务（称为*action*）。作业中的每个步骤都在同一运行程序上执行，从而使该作业中的动作可以彼此共享数据。

### 动作(Actions)

动作是独立的命令，它们被组合成步骤来创建作业。 动作是工作流中最小的可移植构建块。您可以创建自己的动作，也可以使用GitHub社区创建的动作。要在工作流中使用动作，您必须将其包括为一个步骤。

### 运行者(Runners)

运行程序是已安装GitHub Actions运行程序应用程序的服务器。您可以使用GitHub托管的运行程序，也可以托管自己的运行程序。运行程序侦听可用的作业，一次运行一个作业，并将进度，日志和结果报告回GitHub。对于由GitHub托管的运行程序，工作流程中的每个**作业**都在全新的虚拟环境中运行。

GitHub托管的运行程序基于Ubuntu Linux，Microsoft Windows和macOS。有关GitHub托管的运行程序的信息，请参阅“ [GitHub托管的运行程序的虚拟环境](https://docs.github.com/en/free-pro-team@latest/actions/reference/virtual-environments-for-github-hosted-runners)”。如果您需要其他操作系统或需要特定的硬件配置，则可以托管自己的运行程序。有关自托管运行程序的信息，请参阅“[托管您自己的运行程序”](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners)。

## 创建示例工作流程

GitHub Actions使用YAML语法定义事件，作业和步骤。这些YAML文件存储在您的代码存储库中的`.github/workflows`目录中。

您可以在存储库中创建一个示例工作流，每当推送代码时，它都会自动触发一系列命令。 在此工作流程中，GitHub Actions签出推送的代码，安装软件依赖项并运行`bats -v`。

1. 在您的存储库中，创建`.github/workflows/`目录以存储您的工作流文件。

2. 在 `.github/workflows/`  目录中，创建一个名为 `learn-github-actions.yml`  的新文件，并添加以下代码。

   ```yaml
   name: learn-github-actions
   on: [push]
   jobs:
     check-bats-version:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - uses: actions/setup-node@v1
         - run: npm install -g bats
         - run: bats -v
   ```

3. 提交这些更改并将其推送到您的GitHub存储库。

现在，您的新GitHub Actions工作流文件已安装在您的存储库中，并且每次有人将更改推送到存储库时将自动运行。有关作业执行历史的详细信息，请参阅“[查看工作流程的活动”](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions#viewing-the-jobs-activity)。

## 了解工作流程文件

为了帮助您了解如何使用YAML语法创建工作流文件，本节说明了简介示例的每一行：

| 代码      | 说明 |
| ----------------------------------- | ------------------------------------------------------------ |
| `name: learn-github-actions`        | *可选*-工作流的名称，它将显示在GitHub存储库的“动作”选项卡中。 |
| `on: [push]`                        | 指定自动触发工作流文件的事件。本示例使用`push`事件，以便每当有人将更改推送到存储库时作业就运行。您可以将工作流设置为仅在某些分支，路径或标签上运行。有关包括或不包括分支，路径或标签的语法示例，请参阅[“ GitHub Actions的工作流语法”。](https://docs.github.com/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestpaths) |
| `jobs:`                             | 将`learn-github-actions`工作流文件中运行的所有作业分组在一起。 |
| `check-bats-version:`               | 定义存储在`jobs`部分中的`check-bats-version`作业的名称。 |
| `  runs-on: ubuntu-latest`          | 配置作业以在Ubuntu Linux运行器上运行。这意味着该作业将在GitHub托管的新虚拟机上执行。有关使用其他运行程序的语法示例，请参阅[“ GitHub Actions的工作流语法”。](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) |
| `  steps:`                          | 将`check-bats-version`作业中运行的所有步骤组合在一起。嵌套在此部分下的每一行都是单独的动作。 |
| `    - uses: actions/checkout@v2`   | `uses`关键字告诉作业检索名为`actions/checkout@v2`的社区动作的`v2`。此动作将检出您的存储库并将其下载到运行程序，从而使您可以对代码（例如测试工具）运行动作。每当工作流将根据存储库的代码运行时，或者使用存储库中定义的动作时，都必须使用checkout动作。 |
| `    - uses: actions/setup-node@v1` | 该动作将`node`软件包安装在运行程序上，从而使您可以访问`npm`命令。 |
| `    - run: npm install -g bats`    | `run`关键字告诉作业在运行程序上执行命令。在本例中，您使用的是`npm`来安装`bats`软件测试包。 |
| `    - run: bats -v`                | 最后，您将使用参数运行`bats`命令输出软件版本 |

**可视化工作流程文件**

在此图中，您可以看到刚刚创建的工作流文件以及GitHub Actions组件在层次结构中的组织方式。每个步骤执行一个动作。步骤1和2使用预先构建的社区动作。要为您的工作流查找更多的预建动作，请参阅“[查找和自定义动作”](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions)。

![工作流程概述](/overview-actions-event.png)

## 查看作业的活动

作业开始运行后，您可以在GitHub上查看每个步骤的活动。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![导航到存储库](/learn-github-actions-repository.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![工作流程结果的屏幕截图](/learn-github-actions-workflow.png)

   

4. 在“工作流运行”下，单击要查看的运行的名称。

   ![工作流程运行的屏幕截图](/learn-github-actions-run.png)

   

5. 单击作业名称以查看每个步骤的结果。

   ![工作流程运行详细信息的屏幕截图](/overview-actions-result-updated.png)

   

## 下一步

要继续学习GitHub Action，请参阅“[查找和自定义操作”](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions)。



## 参考阅读


