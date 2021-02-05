---
title: 与您的组织共享工作流程
date: 2020-11-01  8:44:27
updated: 2020-11-01  8:44:27
mathjax: false
categories: 
tags:
typora-root-url: sharing-workflows-with-your-organization
typora-copy-images-to: sharing-workflows-with-your-organization
top: 1
comments: false
---


# 与您的组织共享工作流程

[原文](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/sharing-workflows-with-your-organization)

> 了解如何通过共享工作流模板，机密信息和自托管运行者来使用组织功能与团队合作。 



## 总览

如果您需要与团队共享工作流和其他GitHub Actions功能，请考虑在GitHub组织中进行协作。组织允许您集中存储和管理机密，工件和自托管运行程序。您还可以在`.github`资源库中创建工作流程模板，并将其与组织中的其他用户共享。

## 创建工作流程模板

具有对组织的`.github`存储库的写访问权限的用户可以创建工作流模板。然后，有权创建工作流的组织成员可以使用模板。工作流模板可用于在组织的公共存储库中创建新的工作流；要使用模板在私有存储库中创建工作流，组织必须是企业或GitHub One计划的一部分。

此过程演示了如何创建工作流程模板和元数据文件。元数据文件描述了在用户创建新工作流程时如何向用户显示模板。

1. 如果尚不存在，请在您的组织中创建一个新的公共存储库名为`.github`。

2. 创建一个名为`workflow-templates`的目录。

3. 在`workflow-templates`目录内创建新的工作流文件。

   如果需要引用存储库的默认分支，则可以使用`$default-branch`占位符。使用模板创建工作流程时，占位符将自动替换为存储库默认分支的名称。

   例如，此名为`octo-organization-ci.yml`的文件演示了基本的工作流程。

   ```yaml
   name: Octo Organization CI
   
   on:
     push:
       branches: [ $default-branch ]
     pull_request:
       branches: [ $default-branch ]
   
   jobs:
     build:
       runs-on: ubuntu-latest
       
       steps:
       - uses: actions/checkout@v2
       
       - name: Run a one-line script
         run: echo Hello from Octo Organization
   ```

4. 在`workflow-templates`目录内创建一个元数据文件。元数据文件必须与工作流文件具有相同的名称，但必须附加`.properties.json`，而不是扩展名`.yml`。例如，名为`octo-organization-ci.properties.json`的文件包含名为`octo-organization-ci.yml`的工作流程文件的元数据：

   ```json
   {
       "name": "Octo Organization Workflow",
       "description": "Octo Organization CI workflow template.",
       "iconName": "example-icon",
       "categories": [
           "Go"
       ],
       "filePatterns": [
           "package.json$",
           "^Dockerfile",
           ".*\\.md$"
       ]
   }
   ```

   * `name`-**必填。**工作流程模板的名称。这显示在可用模板列表中。
   * `description`-**必填。**工作流程模板的描述。这显示在可用模板列表中。
   * `iconName`-**必填。**为模板列表中的工作流条目定义一个图标。`iconName`必须是同一名称的SVG图标，且必须存放在`workflow-templates`目录中。例如，名为`example-icon.svg`的SVG文件被引用为`example-icon`。
   * `categories`-**可选。**定义工作流的语言类别。当用户查看可用模板时，与相同语言匹配的那些模板将更加突出。有关可用语言类别的信息，请参见[定义GitHub已知的所有语言](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)。
   * `filePatterns`-**可选。**如果用户的存储库在其根目录中具有与定义的正则表达式匹配的文件，则允许使用模板。

要添加另一个工作流程模板，请将文件添加到同一`workflow-templates`目录。例如：

![工作流程模板文件](/workflow-template-files.png)



## 使用工作流程模板

此过程演示组织的成员如何查找和使用工作流程模板来创建新的工作流程。组织的工作流模板可以由组织的任何成员使用。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 如果您的存储库已经具有现有的工作流程：在左上角，点击**新建工作流程**。

   ![创建一个新的工作流程](/actions-new-workflow.png)

4. 组织的工作流模板位于其自己的 “按组织名称创建的工作流”部分中。在要使用的模板名称下，单击“**设置此工作流(Set up this workflow)**”。

   ![设置此工作流程](/actions-create-starter-workflow.png)



## 在组织内共享秘密

您可以集中管理组织内的秘密，然后将其提供给选定的存储库。这也意味着您可以在一个位置更新机密，并将更改应用于使用该机密的所有存储库工作流程。

在组织中创建机密时，可以使用策略来限制哪些存储库可以访问该机密。例如，您可以授予对所有存储库的访问权限，或者将访问权限限制为仅私有存储库或指定的存储库列表。

要在组织级别创建机密，您必须具有`admin`访问权限。

1. 在GitHub上，导航到组织的主页。

2. 在您的组织名称下，点击 **设置**。

   ![img](/organization-settings-tab.png) 

1. 在左侧边栏中，点击**秘密**。
2. 单击“**新机密”**。
3. 在**名称**输入框中输入您的机密**名称**。
4. 输入您的机密**值**。
5. 从“**存储库访问”**下拉列表中，选择访问策略。
6. 单击**添加秘密**。



## 在组织内共享自我托管的运行者

组织管理员可以将其自托管的运行者添加到组中，然后创建策略来控制哪些存储库可以访问该组。

有关更多信息，请参阅“[使用组管理对自托管运行器的访问](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/managing-access-to-self-hosted-runners-using-groups)”。

## 下一步

要继续学习GitHub Action，请参阅“ [GitHub Action的安全性增强](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/security-hardening-for-github-actions)”。



## 参考阅读


