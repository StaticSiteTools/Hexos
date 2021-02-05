---
title: 查找和自定义Actions
date: 2020-10-31  1:24:17
updated: 2020-10-31  1:24:17
mathjax: false
categories: 
tags:
typora-root-url: finding-and-customizing-actions
typora-copy-images-to: finding-and-customizing-actions
top: 1
comments: false
---


# 查找和自定义Actions

[原文](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions)

> 动作是为您的工作流提供动力的基础。工作流可以包含社区创建的动作，也可以直接在应用程序的存储库中创建自己的动作。本指南将向您展示如何发现，使用和自定义动作。 



## 总览

您可以在以下工作流中定义您在工作流中使用的动作：

* 一个公共仓库
* 您的工作流文件引用该动作的同一存储库
* 在Docker Hub上发布的Docker容器映像

GitHub Marketplace是您查找GitHub社区创建的动作的中心位置。[GitHub Marketplace页面](https://github.com/marketplace/actions/)使您可以按类别过滤动作。

## 在工作流编辑器中浏览Marketplace动作

您可以直接在存储库的工作流编辑器中搜索和浏览动作。您可以从边栏中搜索特定动作，查看特色动作并浏览特色类别。您还可以查看从GitHub社区收到的动作数量。

1. 在存储库中，浏览到要编辑的工作流程文件。

2. 在文件视图的右上角，打开，点击![1604079392744](/1604079392744.png)。

   ![img](/actions-edit-workflow-file.png) 

3. 在编辑器的右侧，使用GitHub Marketplace侧栏浏览动作。带有![1604079247118](/1604079247118.png)徽章的动作表明GitHub已验证该动作的创建者为合作伙伴组织。 

   ![img](/actions-marketplace-sidebar.png) 



## 向您的工作流程添加动作

动作的列表页面包括该动作的版本和使用该动作所需的工作流语法。为了即使在对动作进行更新时也能保持工作流程稳定，您可以通过在工作流程文件中指定Git或Docker标记号来引用要使用的动作的版本。

1. 导航到要在工作流中使用的动作。

2. 在“安装”下，单击![1604079448302](/1604079448302.png)以复制工作流程语法。

   ![img](/actions-sidebar-detailed-view.png) 

3. 将语法粘贴为工作流程中的新步骤。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idsteps)”。
4. 如果动作要求您提供输入，请在工作流程中进行设置。有关动作可能需要的输入的信息，请参阅“[将输入和输出与动作一起使用](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions#using-inputs-and-outputs-with-an-action)”。

您还可以为添加到工作流中的动作启用GitHub Dependabot版本更新。有关更多信息，请参阅“[使用GitHub Dependabot保持最新动作](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/keeping-your-actions-up-to-date-with-github-dependabot)”。 



## 使用发布管理进行自定义动作

社区动作的创建者可以选择使用标签，分支或SHA值来管理动作的发布。与任何依赖项类似，您应该根据自己的舒适程度（自动接受动作更新）来指定要使用的动作版本。

您将在工作流文件中指定动作的版本。查看动作的文档以获取有关其发行版管理方法的信息，并查看要使用的标记，分支或SHA值。

### 使用标签

标签对于让您决定何时在主要版本和次要版本之间进行切换很有用，但是这些标记只是暂时的，可以由维护者移动或删除。此示例演示了如何定位标记为`v1.0.1`的动作：

```yaml
steps:
    - uses: actions/javascript-action@v1.0.1
```

### 使用SHA

如果需要更可靠的版本控制，则应使用与动作版本关联的SHA值。SHA是不可变的，因此比标记或分支更可靠。但是，这种方法意味着您将不会自动收到某项动作的更新，包括重要的错误修复和安全更新。本示例以动作的SHA为目标：

```yaml
steps:
    - uses: actions/javascript-action@172239021f7ba04fe7327647b213799853a9eb89
```

### 使用分支

提及特定分支意味着该动作将始终使用目标分支上的最新更新，但是如果这些更新包含重大更改，则可能会产生问题。本示例针对一个名为`@main`的分支：

```yaml
steps:
    - uses: actions/javascript-action@main
```

有关更多信息，请参阅“[对动作使用版本管理](https://docs.github.com/en/free-pro-team@latest/actions/creating-actions/about-actions#using-release-management-for-actions)”。

## 通过动作使用输入和输出

动作通常接受或要求输入，并生成可使用的输出。例如，动作可能需要您指定文件的路径，标签名称或其他将在动作处理过程中使用的数据。

要查看动作的输入和输出，请检查存储库的根目录中的`action.yml`或`action.yaml`。

在此示例中`action.yml`，`inputs`关键字定义了一个称为`file-path`的必需输入，并且包含一个默认值，如果未指定则使用该默认值。`outputs`关键字定义一个名为`results-file`的输出，它告诉你在哪里可以找到结果。

```yaml
name: 'Example'
description: '接收文件并生成输出'
inputs:
  file-path:  # 输入id
    description: "测试脚本的路径"
    required: true
    default: 'test-file.js'
outputs:
  results-file: # 输出id
    description: "结果文件的路径"
```

## 在工作流文件使用动作的同一存储库中引用动作

如果一个动作是在您的工作流程文件使用的动作相同存储库中定义，你可以参考作用无论是`{owner}/{repo}@{ref}`或`./path/to/dir`在您的工作流程文件的语法。

示例存储库文件结构：

```
|-- hello-world (repository)
|   |__ .github
|       └── workflows
|           └── my-first-workflow.yml
|       └── actions
|           |__ hello-world-action
|               └── action.yml
```

工作流程文件示例：

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # 此步骤签出存储库的副本。
      - uses: actions/checkout@v2
      # 此步骤引用包含动作的目录。
      - uses: ./.github/actions/hello-world-action
```

该`action.yml`文件用于提供动作的元数据。了解该文件的内容在 “[GitHub动作的元数据语法](https://docs.github.com/en/free-pro-team@latest/actions/creating-actions/metadata-syntax-for-github-actions)”

## 在Docker Hub上引用容器

如果在Docker Hub上的已发布Docker容器映像中定义了动作，则必须在工作流文件中使用`docker://{image}:{tag}`语法引用该动作。为了保护您的代码和数据，强烈建议您在工作流中使用Docker Hub之前，先验证Docker容器映像的完整性。

```yaml
jobs:
  my_first_job:
    steps:
      - name: My first step
        uses: docker://alpine:3.8
```

有关Docker动作的一些示例，请参阅[Docker-image.yml工作流程](https://github.com/actions/starter-workflows/blob/main/ci/docker-image.yml)和“[创建Docker容器动作”](https://docs.github.com/en/free-pro-team@latest/articles/creating-a-docker-container-action)。

## 下一步

要继续学习GitHub Actions，请参阅“ [GitHub Actions的基本特征](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/essential-features-of-github-actions)”。



## 参考阅读


