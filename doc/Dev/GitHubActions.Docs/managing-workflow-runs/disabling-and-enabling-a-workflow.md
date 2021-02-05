---
title: 禁用和启用工作流程
date: 2020-11-02  0:34:03
updated: 2020-11-02  0:34:03
mathjax: false
categories: 
tags:
typora-root-url: disabling-and-enabling-a-workflow
typora-copy-images-to: disabling-and-enabling-a-workflow
top: 1
comments: false
---


# 禁用和启用工作流程

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/disabling-and-enabling-a-workflow)

> 您可以使用GitHub或REST API禁用和重新启用工作流程。 



禁用工作流程可以使您停止触发工作流程，而不必从存储库中删除文件。您可以在GitHub上轻松轻松地重新启用工作流程。您还可以使用REST API禁用和启用工作流程。有关更多信息，请参阅“ [Actions REST API”](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#workflows)。

在许多情况下，临时禁用工作流可能很有用。这些是禁用工作流程可能会有所帮助的一些示例：

* 产生太多或错误请求的工作流程错误，会对外部服务产生负面影响。
* 工作流程不是很关键，占用您的帐户太多时间。
* 将请求发送到已关闭的服务的工作流。
* 分叉存储库上不需要的工作流（例如计划的工作流）。

> **警告：**为防止不必要的工作流程运行，计划的工作流程可能会自动禁用。派生公共存储库时，默认情况下禁用计划的工作流程。在公共存储库中，如果60天内未发生任何存储库活动，则计划的工作流将自动禁用。



## 禁用工作流程

您可以手动禁用工作流程，以便它不会执行任何工作流程运行。禁用的工作流程不会被删除，可以重新启用。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![img](/actions-tab.png) 

3. 在左侧边栏中，单击要禁用的工作流程。 

   ![img](/actions-select-workflow.png) 

   4. 点击![1604249048477](/1604249048477.png)。 

      ![img](/actions-workflow-menu-kebab.png) 

   5. 点击**禁用工作流程**。 

      ![img](/actions-disable-workflow.png) 

已禁用的工作流程标记为![1604249110989](/1604249110989.png)以指示其状态。 

![img](/actions-find-disabled-workflow.png) 



## 启用工作流程

您可以重新启用以前禁用的工作流程。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 在左侧边栏中，单击要启用的工作流程。

   ![img](/actions-select-disabled-workflow.png) 

4. 点击**启用工作流程**。 

   ![img](/actions-enable-workflow.png) 





## 参考阅读


