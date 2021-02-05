---
title: 关于持续集成
date: 2020-11-03  7:20:56
updated: 2020-11-03  7:20:56
mathjax: false
categories: 
tags:
typora-root-url: about-continuous-integration
typora-copy-images-to: about-continuous-integration
top: 1
comments: false
---


# 关于持续集成

[原文](https://docs.github.com/en/free-pro-team@latest/actions/guides/about-continuous-integration)

> 您可以使用GitHub Actions在GitHub存储库中直接创建自定义的持续集成（CI）和持续部署（CD）工作流。 



## 持续集成

持续集成（CI）是一种软件实践，需要经常将代码提交到共享存储库。提交代码的频率更高，可以更快地发现错误，并减少了开发人员在查找错误源时需要调试的代码量。频繁的代码更新还使合并来自软件开发团队不同成员的更改更加容易。这对于开发人员来说非常有用，他们可以将更多的时间花在编写代码上，而将花费在调试错误或解决合并冲突上的时间减少。

将代码提交到存储库时，可以连续构建和测试代码，以确保提交不会引入错误。您的测试可以包括代码短绒（检查样式格式），安全性检查，代码覆盖率，功能测试以及其他自定义检查。

构建和测试代码需要服务器。您可以在将代码推送到存储库之前在本地构建和测试更新，也可以使用CI服务器检查存储库中的新代码提交。

## 使用GitHub Actions进行持续集成

使用GitHub Actions的CI提供了可以在您的存储库中构建代码并运行测试的工作流。工作流可以在GitHub托管的虚拟机上运行，也可以在您自己托管的计算机上运行。有关更多信息，请参阅“[适用于GitHub托管的运行器的虚拟环境](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/virtual-environments-for-github-hosted-runners)”和“[关于自托管运行器](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/about-self-hosted-runners)”。

您可以将CI工作流配置为在发生GitHub事件（例如，将新代码推送到您的存储库）时，按设定的时间表运行或在发生外部事件时使用存储库调度Webhook运行。

GitHub运行您的CI测试，并在pull请求中提供每个测试的结果，因此您可以查看分支中的更改是否引入了错误。当工作流程中的所有CI测试通过时，您推送的更改已准备好由团队成员进行审核或合并。如果测试失败，则可能是您所做的更改之一导致了失败。

在存储库中设置CI时，GitHub会分析存储库中的代码，并根据存储库中的语言和框架推荐CI工作流。例如，如果您使用[Node.js](https://nodejs.org/en/)，则GitHub将建议一个模板文件来安装您的Node.js包并运行测试。您可以使用GitHub建议的CI工作流程模板，自定义建议的模板或创建自己的自定义工作流程文件来运行CI测试。

![建议的持续集成模板的屏幕截图](/ci-with-actions-template-picker.png)

除了帮助您为项目设置CI工作流之外，您还可以使用GitHub Actions在整个软件开发生命周期中创建工作流。例如，您可以使用动作来部署，打包或发布项目。有关更多信息，请参阅“[关于GitHub动作”](https://docs.github.com/en/free-pro-team@latest/articles/about-github-actions)。

有关常用术语的定义，请参阅“ [GitHub Actions的核心概念](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/core-concepts-for-github-actions)”。

## 支持的语言

GitHub提供了适用于多种语言和框架的CI工作流模板。

在[action / starter-workflows](https://github.com/actions/starter-workflows/tree/main/ci)存储库中浏览GitHub提供的CI工作流模板的完整列表。

## 工作流程运行通知

如果为GitHub Actions启用电子邮件或Web通知，则当您触发的任何工作流运行完成时，您将收到一条通知。该通知将包括工作流程运行的状态（包括成功，失败，中立和已取消的运行）。您还可以选择仅在工作流运行失败时才接收通知。

您还可以在存储库的“动作”选项卡上查看工作流运行的状态。有关更多信息，请参阅“[管理工作流运行”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/managing-a-workflow-run)。

## 工作流程运行的状态标志

状态标记显示工作流当前是失败还是通过。添加状态标记的常见位置是存储库的README.md文件中，但您可以将其添加到所需的任何网页中。默认情况下，标志显示默认分支的状态。您还可以使用URL中的`branch`和`event`查询参数显示特定分支或事件的工作流程运行状态。

![状态徽章示例](/actions-workflow-status-badge.png)

有关更多信息，请参见“[配置工作流程”](https://docs.github.com/en/free-pro-team@latest/articles/configuring-a-workflow)。

## 进一步阅读

* “[使用GitHub Actions建立持续集成](https://docs.github.com/en/free-pro-team@latest/articles/setting-up-continuous-integration-using-github-actions)”
* “[管理GitHub动作的账单](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/managing-billing-for-github-actions)”



## 参考阅读


