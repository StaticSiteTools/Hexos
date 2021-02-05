---
title: 添加工作流状态标志
date: 2020-11-02 17:49:17
updated: 2020-11-02 17:49:17
mathjax: false
categories: 
tags:
typora-root-url: adding-a-workflow-status-badge
typora-copy-images-to: adding-a-workflow-status-badge
top: 1
comments: false
---


# 添加工作流状态标志

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/adding-a-workflow-status-badge)

> 您可以在存储库中显示状态标志，以指示工作流程的状态。 



状态标记显示工作流当前是失败还是通过。添加状态标记的常见位置是存储库的README.md文件中，但您可以将其添加到所需的任何网页中。默认情况下，标志显示默认分支的状态。您还可以使用URL中的`branch`和`event`查询参数显示特定分支或事件的工作流运行状态。

![状态徽章示例](/actions-workflow-status-badge.png)

如果您的工作流使用`name`关键字，则必须按名称引用工作流。如果工作流的名称包含空格，则需要用URL编码的string替换空格`%20`。有关`name`关键字的更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions#name)”。

```
https://github.com/<OWNER>/<REPOSITORY>/workflows/<WORKFLOW_NAME>/badge.svg
```

或者，如果您的工作流程没有`name`，则必须使用相对于存储库根目录的文件路径引用工作流程文件。

> **注意：**如果工作流程包含`name`，则无法使用文件路径引用工作流程文件。

```
https://github.com/<OWNER>/<REPOSITORY>/workflows/<WORKFLOW_FILE_PATH>/badge.svg
```

## 使用工作流程名称

这个Markdown示例为名称为“ Greet Everyone”的工作流添加了状态徽章。该仓库的`OWNER`是`actions`组织和`REPOSITORY`名字是`hello-world`。

```
![example workflow name](https://github.com/actions/hello-world/workflows/Greet%20Everyone/badge.svg)
```

## 使用工作流程文件路径

这个Markdown示例为带有文件路径`.github/workflows/main.yml`的工作流添加了状态标记。该仓库的`OWNER`是`actions`组织和`REPOSITORY`名字是`hello-world`。

```
![example workflow file path](https://github.com/actions/hello-world/workflows/.github/workflows/main.yml/badge.svg)
```

## 使用`branch`参数

这个Markdown示例为名称为`feature-1`的分支添加了状态徽章。

```
![example branch parameter](https://github.com/actions/hello-world/workflows/Greet%20Everyone/badge.svg?branch=feature-1)
```

## 使用`event`参数

这个Markdown示例添加了一个徽章，用于显示由`pull_request`事件触发的工作流运行状态。

```
![example event parameter](https://github.com/actions/hello-world/workflows/Greet%20Everyone/badge.svg?event=pull_request)
```





## 参考阅读


