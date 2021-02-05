---
title: 使用工作流程模板设置持续集成
date: 2020-11-03  7:30:13
updated: 2020-11-03  7:30:13
mathjax: false
categories: 
tags:
typora-root-url: setting-up-continuous-integration-using-workflow-templates
typora-copy-images-to: setting-up-continuous-integration-using-workflow-templates
top: 1
comments: false
---


# 使用工作流程模板设置持续集成

[原文](https://docs.github.com/en/free-pro-team@latest/actions/guides/setting-up-continuous-integration-using-workflow-templates)

> 您可以使用与您要使用的语言和工具相匹配的工作流模板为项目设置持续集成。 



具有存储库写权限的任何人都可以使用GitHub Actions设置持续集成（CI）。

设置CI后，您可以自定义工作流程以满足您的需求。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 找到与您要使用的语言和工具相匹配的模板，然后点击 **设置此工作流程**。

   ![设置工作流程按钮](/setup-workflow-button.png)

4. 单击**开始提交**。

   ![开始提交按钮](/start-commit.png)

5. 在页面底部，键入简短的，有意义的提交消息，以描述您对该文件所做的更改。您可以在提交消息中将提交归因于多个作者。有关更多信息，请参见“[创建具有多个共同作者的提交”](https://docs.github.com/en/free-pro-team@latest/articles/creating-a-commit-with-multiple-authors)。 

   ![提交您的更改消息](/write-commit-message-quick-pull.png)

6. 在提交消息字段下方，确定是将提交添加到当前分支还是新分支。如果当前分支是默认分支，则应选择为提交创建一个新分支，然后创建一个拉取请求。有关更多信息，请参见“[创建新的拉取请求”](https://docs.github.com/en/free-pro-team@latest/articles/creating-a-pull-request)。 

   ![提交分支选项](/choose-commit-branch.png)

7. 单击**提议新文件。** 

   ![提议新文件按钮](/new-file-commit-button.png)

推送到存储库后，您可以跟踪在GitHub上运行的持续集成工作流的状态和详细日志，并接收自定义的通知。有关更多信息，请参阅“[配置通知](https://docs.github.com/en/free-pro-team@latest/github/managing-subscriptions-and-notifications-on-github/configuring-notifications#github-actions-notification-options)”和“[管理工作流运行”](https://docs.github.com/en/free-pro-team@latest/articles/managing-a-workflow-run)。

状态标记显示工作流当前是失败还是通过。添加状态标记的常见位置是存储库的README.md文件中，但您可以将其添加到所需的任何网页中。默认情况下，标志显示默认分支的状态。您还可以使用URL中的`branch`和`event`查询参数显示特定分支或事件的工作流程运行状态。

![状态徽章示例](/actions-workflow-status-badge.png)

有关更多信息，请参见“[学习GitHub操作”](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions)。



**进一步阅读**

* “[关于持续集成](https://docs.github.com/en/free-pro-team@latest/articles/about-continuous-integration)”
* “[管理工作流运行](https://docs.github.com/en/free-pro-team@latest/articles/managing-a-workflow-run)”
* “[管理GitHub动作的账单](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/managing-billing-for-github-actions)”



## 参考阅读


