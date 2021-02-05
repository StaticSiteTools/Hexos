---
title: 手动运行工作流程
date: 2020-11-02 19:08:15
updated: 2020-11-02 19:08:15
mathjax: false
categories: 
tags:
typora-root-url: manually-running-a-workflow
typora-copy-images-to: manually-running-a-workflow
top: 1
comments: false
---


# 手动运行工作流程

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/manually-running-a-workflow)

> 将工作流程配置为在`workflow_dispatch`事件上运行时，可以使用REST API或从GitHub上的“操作”选项卡运行工作流程。 



要手动运行工作流程，必须将工作流程配置为在`workflow_dispatch`事件上运行。有关更多信息，请参阅“[触发工作流的事件](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)”。

## 在GitHub上运行工作流程

要在GitHub上触发`workflow_dispatch`事件，您的工作流程必须在默认分支中。请按照以下步骤手动触发工作流程运行。

执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 在左侧边栏中，单击要运行的工作流程。

   ![动作选择工作流程](/actions-select-workflow.png)

4. 在工作流程运行列表上方，选择**运行工作流程**。

   ![动作工作流调度](/actions-workflow-dispatch.png)

5. 选择工作流将在其中运行的分支，然后键入工作流使用的输入参数。点击**运行工作流程**。

   ![动作手动运行工作流程](/actions-manually-run-workflow.png)

## 使用REST API运行工作流程

使用REST API时，您可以配置`inputs`和`ref`作为请求主体参数。如果省略输入，则使用工作流文件中定义的默认值。

有关使用REST API的更多信息，请参阅“[创建工作流调度事件”](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions/#create-a-workflow-dispatch-event)。



## 参考阅读


