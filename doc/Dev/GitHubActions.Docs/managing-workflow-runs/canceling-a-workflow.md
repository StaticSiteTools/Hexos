---
title: 取消工作流程
date: 2020-11-02 19:22:24
updated: 2020-11-02 19:22:24
mathjax: false
categories: 
tags:
typora-root-url: canceling-a-workflow
typora-copy-images-to: canceling-a-workflow
top: 1
comments: false
---


# 取消工作流程

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/canceling-a-workflow)

> 您可以取消正在进行的工作流程运行。取消工作流程运行时，GitHub会取消该工作流程中的所有作业和步骤。 



执行这些步骤需要对存储库进行写访问。

## 取消工作流程运行

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/workflow-sidebar.png)

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/run-name.png)

5. 在工作流程的右上角，点击**取消工作流程**。

   ![取消检查套件按钮](/cancel-check-suite.png)

## GitHub取消工作流程运行的步骤

取消工作流程运行时，您可能正在运行使用与该工作流程运行相关的资源的其他软件。为了帮助您释放与工作流程运行相关的资源，它可能有助于了解GitHub为取消工作流程运行所执行的步骤。

1. 要取消工作流运行，服务器将重新评估所有当前正在运行的作业的`if`条件。如果条件计算为`true`，则作业不会被取消。例如，条件`if: always()`将评估为true，并且作业将继续运行。如果没有条件，则相当于条件`if: success()`，条件只有在上一步成功完成时才运行。
2. 对于需要取消的作业，服务器将取消消息发送到所有需要取消的作业的运行计算机。
3. 对于继续运行的作业，服务器将重新评估未完成步骤的`if`条件。如果条件评估为`true`，则该步骤继续运行。
4. 对于需要取消的步骤，运行程序机器将发送`SIGINT/Ctrl-C`到步骤的输入过程（`node`用于javascript动作，`docker`用于容器动作以及`bash/cmd/pwd`在步骤中使用`run`时）。如果该流程未在7500毫秒内退出，则运行程序将发送`SIGTERM/Ctrl-Break`至该流程，然后等待2500毫秒让该流程退出。如果进程仍在运行，则运行程序将终止进程树。
5. 在5分钟的取消超时期限之后，服务器将强制终止所有未完成运行或无法完成取消过程的作业和步骤。



## 参考阅读


