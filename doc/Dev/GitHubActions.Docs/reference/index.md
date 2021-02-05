---
title: 参考
date: 2020-11-03  7:44:55
updated: 2020-11-03  7:44:55
mathjax: false
categories: 
tags:
typora-root-url: index
typora-copy-images-to: index
top: 1
comments: false
---


# 参考

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference)

> 用于创建工作流，使用GitHub托管的运行器和身份验证的参考文档。 



## 工作流程语法

工作流程文件使用YAML编写。在YAML工作流文件中，可以使用表达式语法来评估上下文信息，文字，运算符和函数。上下文信息包括工作流，环境变量，机密以及触发工作流的事件。当使用工作流中的步骤来运行shell命令，则可以使用特定的工作流程命令语法[`run`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsrun)来设置环境变量，用于随后的步骤设定的输出参数，以及一组错误或调试消息。

* [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions)   [md](workflow-syntax-for-github-actions.md)
* [GitHub Actions的上下文和表达语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)    [md](context-and-expression-syntax-for-github-actions.md)
* [GitHub Actions的工作流程命令](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-commands-for-github-actions)    [md](workflow-commands-for-github-actions.md)

## 事件

您可以将工作流配置为在特定的GitHub事件发生时，在计划的时间，手动或在GitHub以外的事件发生时运行。

* [触发工作流程的事件](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)   [md](events-that-trigger-workflows.md)

## 身份验证和机密

GitHub提供了一个令牌，您可以使用该令牌代表GitHub Actions进行身份验证。您还可以将敏感信息作为秘密存储在组织或存储库中。GitHub加密所有秘密。

* [工作流程中的身份验证](https://docs.github.com/en/free-pro-team@latest/actions/reference/authentication-in-a-workflow)  [md](authentication-in-a-workflow.md)
* [加密机密](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets)     [md](encrypted-secrets.md)

## GitHub托管的运行者

GitHub提供了托管虚拟机来运行工作流。虚拟机包含一个环境，其中包含工具，软件包和环境变量，供GitHub Actions使用。

* [环境变量](https://docs.github.com/en/free-pro-team@latest/actions/reference/environment-variables)   [md](environment-variables.md)
* [GitHub托管的运行者规格](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners)  [md](specifications-for-github-hosted-runners.md)

## 行政

在GitHub托管的运行器上运行工作流时，存在使用限制和潜在的使用费用。您还可以在存储库和组织中禁用或限制GitHub Actions的使用。

* [使用限制，计费和管理](https://docs.github.com/en/free-pro-team@latest/actions/reference/usage-limits-billing-and-administration)   [md](usage-limits-billing-and-administration.md)



## 参考阅读


