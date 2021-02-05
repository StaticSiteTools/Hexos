---
title: 查看作业执行时间
date: 2020-11-02 20:26:01
updated: 2020-11-02 20:26:01
mathjax: false
categories: 
tags:
typora-root-url: viewing-job-execution-time
typora-copy-images-to: viewing-job-execution-time
top: 1
comments: false
---


# 查看作业执行时间

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/viewing-job-execution-time)

> 您可以查看作业的执行时间，包括该作业的可计费分钟数。 



仅针对在使用GitHub托管的运行器的私有存储库上运行的作业显示可计费的作业执行分钟数。在公共存储库中使用GitHub Actions或在自托管运行器上运行的作业时，无需计费。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/run-name.png)

   

5. 在作业摘要下，您可以查看作业的执行时间。要查看计费作业执行时间，请点击**运行和计费时间详细信息**。

   ![运行时间和计费时间详细信息链接](/view-run-billable-time.png)

   

   > **注意：**所示的可计费时间不包括任何舍入或分钟乘数。要查看GitHub Actions的总使用情况，包括四舍五入和分钟乘数，请参阅“[查看GitHub Actions的使用”](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/viewing-your-github-actions-usage)。



## 参考阅读


