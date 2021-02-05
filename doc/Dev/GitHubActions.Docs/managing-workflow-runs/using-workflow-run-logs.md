---
title: 使用工作流程运行日志
date: 2020-11-02 18:05:22
updated: 2020-11-02 18:05:22
mathjax: false
categories: 
tags:
typora-root-url: using-workflow-run-logs
typora-copy-images-to: using-workflow-run-logs
top: 1
comments: false
---


# 使用工作流程运行日志

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/using-workflow-run-logs)

> 您可以在工作流程运行中查看，搜索和下载每个作业的日志。 



您可以从工作流运行页面查看工作流运行是正在进行还是已完成。您必须登录GitHub帐户才能查看工作流程运行信息，包括公共存储库。有关更多信息，请参阅“ [GitHub上的访问权限](https://docs.github.com/en/free-pro-team@latest/articles/access-permissions-on-github)”。

如果运行完成，则可以查看结果是成功，失败，已取消还是中立。如果运行失败，则可以查看和搜索构建日志以诊断失败并重新运行工作流。您还可以查看可计费的作业执行分钟，或下载日志并构建工件。

GitHub Actions使用Checks API输出工作流的状态，结果和日志。GitHub为每个工作流程运行创建一个新的检查套件。检查套件包含针对工作流程中每个作业的检查运行，并且每个作业都包含步骤。GitHub Actions作为工作流中的一个步骤运行。 有关Checks API的更多信息，请参见“ [Checks”](https://docs.github.com/en/free-pro-team@latest/v3/checks)。

> **注意：**确保仅将有效的工作流文件提交到存储库。如果`.github/workflows`包含无效的工作流文件，则GitHub Actions会为每个新提交生成失败的工作流运行。

## 查看日志以诊断故障

如果您的工作流运行失败，则可以查看导致失败的步骤，并查看失败步骤的构建日志以进行故障排除。您可以看到每个步骤运行所花费的时间。您还可以复制日志文件中特定行的永久链接，以与团队共享。执行这些步骤需要对存储库具有读取权限。

除了在工作流文件中配置的步骤外，GitHub还向每个作业添加了两个附加步骤，以设置和完成作业的执行。这些步骤以名称“设置作业”和“完成作业”记录在工作流中。

对于在GitHub上托管的运行程序上运行的作业，“设置作业”记录了运行程序的虚拟环境的详细信息，并包括指向运行程序计算机上存在的预安装工具列表的链接。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/superlinter-workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/superlinter-run-name.png)

   

5. 在左侧边栏中，单击要查看的作业。

   ![Lint代码基础作业](/superlinter-lint-code-base-job.png)

   

6. 任何失败的步骤都会自动展开以显示结果。

   ![超级皮棉工作流程结果](/super-linter-workflow-results-updated.png)

   

7. （可选）要获得指向日志中特定行的链接，请单击步骤的行号。然后，您可以从Web浏览器的地址栏中复制链接。

   ![复制链接的按钮](/copy-link-button-updated.png)

   

## 搜索日志

您可以在构建日志中搜索特定步骤。搜索日志时，结果中仅包含扩展步骤。执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/superlinter-workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/superlinter-run-name.png)

   

5. 在左侧边栏中，单击要查看的作业。

   ![Lint代码基础作业](/superlinter-lint-code-base-job.png)

   

6. 在日志输出的右上角的“**搜索日志”**搜索框中，键入搜索查询。

   ![搜索框搜索日志](/search-log-box-updated.png)

   

## 下载日志

您可以从工作流程运行中下载日志文件。您还可以下载工作流程的工件。有关更多信息，请参见“[使用工件持久化工作流数据](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/persisting-workflow-data-using-artifacts)”。执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/superlinter-workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/superlinter-run-name.png)

   

5. 在左侧边栏中，单击要查看的作业。

   ![Lint代码基础作业](/superlinter-lint-code-base-job.png)

   

6. 点击右上角的，然后选择**下载日志存档**。

   ![下载日志下拉菜单](/download-logs-drop-down-updated.png)

   

## 删除日志

您可以从工作流运行中删除日志文件。执行这些步骤需要对存储库进行写访问。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/superlinter-workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/superlinter-run-name.png)

   

5. 点击右上角的。

   ![烤肉串水平图标](/workflow-run-kebab-horizontal-icon-updated.png)

   

6. 要删除日志文件，请单击“**删除所有日志”**按钮，然后查看确认提示。

   ![删除所有日志](/delete-all-logs-updated.png)

   删除日志后，将移除“**删除所有日志**”按钮，以指示工作流运行中没有剩余日志文件。





## 参考阅读


