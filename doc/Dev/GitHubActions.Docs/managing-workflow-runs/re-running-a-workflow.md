---
title: 重新运行工作流程
date: 2020-11-02 19:19:16
updated: 2020-11-02 19:19:16
mathjax: false
categories: 
tags:
typora-root-url: re-running-a-workflow
typora-copy-images-to: re-running-a-workflow
top: 1
comments: false
---


# 重新运行工作流程

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/re-running-a-workflow)

> 您可以重新运行工作流程的实例。重新运行工作流程使用与触发工作流程运行的原始事件相同的`GITHUB_SHA`（提交SHA）和`GITHUB_REF`（Git ref）。 



执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/workflow-sidebar.png)

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/run-name.png)

5. 在工作流程的右上角，使用**重新运行作业**下拉菜单，然后选择**重新运行所有作业**。 

   ![重新运行检查下拉菜单](/rerun-checks-drop-down.png)



## 参考阅读


