---
title: GitHub Actions快速入门
date: 2020-10-30 22:47:01
updated: 2020-10-30 22:47:01
mathjax: false
categories: 
tags:
typora-root-url: quickstart
typora-copy-images-to: quickstart
top: 1
comments: false
---


# GitHub Actions快速入门

[原文](https://docs.github.com/en/free-pro-team@latest/actions/quickstart)



## 介绍

您只需要现有的GitHub存储库即可创建和运行GitHub Actions工作流程。在本指南中，您将添加使用[GitHub Super-Linter action](https://github.com/github/super-linter)减少多种编码语言的工作流。每次将新提交推送到存储库时，工作流均使用Super-Linter来验证源代码。 

## 创建您的第一个工作流

1. 在GitHub上的存储库中，在名为`.github/workflows`的目录中创建一个新文件`superlinter.yml`。

2. 将以下YAML内容复制到`superlinter.yml`文件中。**注意：**如果您的默认分支不是`main`，请更新`DEFAULT_BRANCH`的值以匹配存储库的默认分支名称。

   ```yaml
   name: Super-Linter
   
   # 每次将新提交推送到存储库时运行此工作流
   on: push
   
   jobs:
     # 设置作业(任务)键。如果未提供作业名称，则键将显示为作业名称
     super-lint:
       # 为作业(任务)命名
       name: Lint code base
       # 设置要运行的机器类型
       runs-on: ubuntu-latest
   
       steps:
         # 在ubuntu最新的机器上检出你的存储库副本
         - name: Checkout code
           uses: actions/checkout@v2
   
         # 运行 Super-Linter 动作
         - name: Run Super-Linter
           uses: github/super-linter@v3
           env:
             DEFAULT_BRANCH: main
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
   ```

3. 要运行您的工作流，请滚动至页面底部，然后选择**为此提交创建新分支并启动拉取请求**。然后，要创建拉取请求，请点击**提出新文件(Propose new file)**。

   ![img](/commit-workflow-file.png)  

在存储库中提交工作流文件将触发`push`事件并运行您的工作流。 



## 查看您的工作流结果

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![img](/actions-tab.png) 

3. 在左侧边栏中，单击要查看的工作流。 

   ![img](/superlinter-workflow-sidebar.png) 

4. 从工作流运行的列表中，单击要查看的运行的名称。 

   ![img](/superlinter-run-name.png) 

5. 在左侧边栏中，单击**Lint code base**作业(任务)。

    ![img](/superlinter-lint-code-base-job.png) 

6. 任何失败的步骤都会自动展开以显示结果。 

   ![img](/super-linter-workflow-results-updated.png) 





## 更多初学者工作流

GitHub提供了预配置的工作流模板，您可以从中开始进行自动化或创建连续集成工作流。您可以在[action / starter-workflows](https://github.com/actions/starter-workflows)存储库中浏览工作流程模板的完整列表。 



## 下一步

只要将代码推送到存储库中，您添加的超级线性工作流就可以运行，以帮助您发现代码中的错误和不一致之处。但是，这仅仅是您可以使用GitHub Actions进行的开始。您的存储库可以包含多个工作流，这些工作流根据不同的事件触发不同的作业。GitHub Actions可以帮助您自动化应用程序开发流程的几乎每个方面。准备开始了吗？以下是一些有用的资源，可帮助您进一步执行GitHub Actions：

* “[学习GitHub动作](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions)”以获得深入的教程
* 特定用例和示例的“[指南](https://docs.github.com/en/free-pro-team@latest/actions/guides)”
* [github/super-linter](https://github.com/github/super-linter)  了解有关配置Super-Linter动作的更多详细信息





## 参考阅读


