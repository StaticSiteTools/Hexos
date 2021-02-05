---
title: 环境变量
date: 2020-11-07  7:22:20
updated: 2020-11-07  7:22:20
mathjax: false
categories: 
tags:
typora-root-url: environment-variables
typora-copy-images-to: environment-variables
top: 1
comments: false
---


# 环境变量

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/environment-variables)

> GitHub为每个GitHub Actions工作流程运行设置默认环境变量。您还可以在工作流文件中设置自定义环境变量。 



## 关于环境变量

GitHub设置默认环境变量，可用于工作流运行的每个步骤。环境变量区分大小写。在动作或步骤中运行的命令可以创建、读取和修改环境变量。 

可以使用[`jobs.<job_id>.steps.env`](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idstepsenv) 、[`jobs.<job_id>.env`](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idenv) 和[`env`](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#env)  关键字为步骤、作业或整个工作流定义环境变量。有关更多信息，请参阅“ [GitHub的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions/#jobsjob_idstepsenv)”。

```yaml
steps:
  - name: Hello world
    run: echo Hello world $FIRST_NAME $middle_name $Last_Name!
    env:
      FIRST_NAME: Mona
      middle_name: The
      Last_Name: Octocat
```

您还可以使用`GITHUB_ENV`环境文件来设置环境变量，工作流中的步骤可以使用以下环境变量。该环境文件可以由动作直接使用，也可以使用`run`关键字作为工作流文件中的Shell命令使用。有关更多信息，请参阅“ [GitHub Actions的工作流命令](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-commands-for-github-actions/#setting-an-environment-variable)”。

## 默认环境变量

我们强烈建议动作使用环境变量来访问文件系统，而不是使用硬编码的文件路径。GitHub设置环境变量，以便在所有运行程序环境中使用动作。

| 环境变量             | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| `CI`                 | 始终设置为`true`。                                           |
| `GITHUB_WORKFLOW`    | 工作流程的名称。                                             |
| `GITHUB_RUN_ID`      | 存储库中每次运行的唯一编号。如果您重新运行工作流运行，则此数字不会更改。 |
| `GITHUB_RUN_NUMBER`  | 存储库中特定工作流程的每次运行的唯一编号。对于工作流程的第一次运行，此数字从1开始，并在每次新运行时递增。如果您重新运行工作流运行，则此数字不会更改。 |
| `GITHUB_ACTION`      | 动作的唯一标识符（`id`）。                                   |
| `GITHUB_ACTIONS`     | 始终设置为`true`当GitHub Actions运行工作流程时。您可以使用此变量来区分何时在本地或通过GitHub Actions运行测试。 |
| `GITHUB_ACTOR`       | 启动工作流程的人员或应用程序的名称。例如，`octocat`。        |
| `GITHUB_REPOSITORY`  | 所有者和存储库名称。例如，`octocat/Hello-World`。            |
| `GITHUB_EVENT_NAME`  | 触发工作流的webhook事件的名称。                              |
| `GITHUB_EVENT_PATH`  | 具有完整的webhook事件有效负载的文件的路径。例如，`/github/workflow/event.json`。 |
| `GITHUB_WORKSPACE`   | GitHub工作区目录路径。如果您的工作流使用 [actions/checkout](https://github.com/actions/checkout)  动作，则工作空间目录是您存储库的副本。如果您不使用该`actions/checkout`动作，则目录将为空。例如，`/home/runner/work/my-repo-name/my-repo-name`。 |
| `GITHUB_SHA`         | 触发工作流的提交SHA。例如，`ffac537e6cbbf934b08745a378932722df287a53`。 |
| `GITHUB_REF`         | 触发工作流的分支或标记ref。例如，`refs/heads/feature-branch-1`。如果事件类型没有分支或标记，则该变量将不存在。 |
| `GITHUB_HEAD_REF`    | 仅设置用于 forked  存储库。头存储库的分支。                  |
| `GITHUB_BASE_REF`    | 仅设置用于 forked  存储库。基础存储库的分支。                |
| `GITHUB_SERVER_URL`  | 返回GitHub服务器的URL。例如：`https://github.com`。          |
| `GITHUB_API_URL`     | 返回API URL。例如：`https://api.github.com`。                |
| `GITHUB_GRAPHQL_URL` | 返回GraphQL API URL。例如：`https://api.github.com/graphql`。 |

## 环境变量的命名约定

> **注意：** GitHub保留环境变量前缀`GITHUB_`供GitHub内部使用。使用`GITHUB_`前缀设置环境变量或密钥将导致错误。

您设置的指向文件系统上某个位置的任何新环境变量都应带有`_PATH`后缀。`HOME`和`GITHUB_WORKSPACE`缺省变量是例外约定，因为字“home”和“workspace”已暗示的位置。



## 参考阅读


