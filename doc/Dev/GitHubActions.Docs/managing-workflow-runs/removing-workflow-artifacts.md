---
title: 移除工作流工件
date: 2020-11-02 21:58:44
updated: 2020-11-02 21:58:44
mathjax: false
categories: 
tags:
typora-root-url: removing-workflow-artifacts
typora-copy-images-to: removing-workflow-artifacts
top: 1
comments: false
---


# 移除工作流工件

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/removing-workflow-artifacts)

> 您可以通过在工件在GitHub上过期之前删除工件来回收使用的GitHub Actions存储。 



## 删除工件

> **警告：**删除工件后，将无法还原它。

执行这些步骤需要对存储库进行写访问。

默认情况下，GitHub将构建日志和工件存储90天，并且该保留期限可以自定义。有关更多信息，请参见“[使用限制，计费和管理](https://docs.github.com/en/free-pro-team@latest/actions/reference/usage-limits-billing-and-administration#artifact-and-log-retention-policy)”。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![img](/actions-tab.png) 

3. 在左侧边栏中，单击要查看的工作流程。  

   ![img](/workflow-sidebar.png) 

4. 从工作流运行的列表中，单击要查看的运行的名称。 

   ![img](/run-name.png) 

5. 在“**工件**”下，单击要删除的工件旁边的![1604325783147](/1604325783147.png)。 

   ![img](/actions-delete-artifact.png) 

   

## 设置工件的保留期限

可以在存储库，组织和企业级别配置工件和日志的保留期。有关更多信息，请参阅“[使用限制，计费和管理”](https://docs.github.com/en/free-pro-team@latest/actions/reference/usage-limits-billing-and-administration#artifact-and-log-retention-policy)。

您还可以使用工作流中的`actions/upload-artifact`动作为单个工件定义自定义保留期。有关更多信息，请参阅“[将工作流数据存储为工件”](https://docs.github.com/en/free-pro-team@latest/actions/guides/storing-workflow-data-as-artifacts#configuring-a-custom-artifact-retention-period)。

## 查找工件的到期日期

您可以使用API确认计划删除工件的日期。有关更多信息，请参阅“[列出存储库的工件](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#artifacts)”  `expires_at`返回的值。



## 参考阅读


