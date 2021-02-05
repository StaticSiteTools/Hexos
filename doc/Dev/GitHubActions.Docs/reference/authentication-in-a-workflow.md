---
title: 工作流程中的身份验证
date: 2020-11-05 19:27:07
updated: 2020-11-05 19:27:07
mathjax: false
categories: 
tags:
typora-root-url: authentication-in-a-workflow
typora-copy-images-to: authentication-in-a-workflow
top: 1
comments: false
---


# 工作流程中的身份验证

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/authentication-in-a-workflow)

> GitHub提供了一个令牌，您可以使用该令牌代表GitHub Actions进行身份验证。 



有`write`权访问存储库的任何人都可以创建，读取和使用机密。

## 关于`GITHUB_TOKEN`机密

GitHub自动创建一个可在您的工作流程中使用的`GITHUB_TOKEN`机密。您可以在工作流运行中使用`GITHUB_TOKEN`进行身份验证。

启用GitHub Actions后，GitHub会在您的存储库中安装GitHub App。该`GITHUB_TOKEN`机密是GitHub的应用程序安装的访问令牌。您可以使用安装访问令牌代表存储库中安装的GitHub App进行身份验证。令牌的权限仅限于包含您的工作流程的存储库。有关更多信息，请参见[`GITHUB_TOKEN`权限](https://docs.github.com/en/free-pro-team@latest/actions/reference/authentication-in-a-workflow#permissions-for-the-github_token)。

在每个作业开始之前，GitHub会获取该作业的安装访问令牌。作业完成后，令牌将过期。

该令牌在`github.token`上下文中也可用。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#github-context)”。

## 使用`GITHUB_TOKEN`工作流

要使用`GITHUB_TOKEN`机密，必须在工作流程文件中引用它。使用令牌可能包括将令牌作为输入传递给需要它的动作，或者进行经过身份验证的GitHub API调用。

当您使用存储库的`GITHUB_TOKEN`代表GitHub Actions应用程序执行任务时，由`GITHUB_TOKEN`触发的事件将不会创建新的工作流程运行。这样可以防止您意外创建递归工作流运行。例如，如果工作流运行使用存储库的`GITHUB_TOKEN`推送代码，即使存储库包含配置为在`push`事件发生时运行的工作流，新的工作流也不会运行。

### `GITHUB_TOKEN`作为输入传递的示例

此示例工作流使用[labeler action](https://github.com/actions/labeler)，它需要使用`GITHUB_TOKEN`作为`repo-token`输入参数的值：

```yaml
name: Pull request labeler
on:
- pull_request
jobs:
  triage:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/labeler@v2
      with:
        repo-token: ${{ secrets.GITHUB_TOKEN }}
```

### 调用REST API的示例

您可以使用`GITHUB_TOKEN`进行经过身份验证的API调用。此示例工作流使用GitHub REST API创建了一个问题：

```yaml
name: Create issue on commit
on:
- push
jobs:
  create_commit:
    runs-on: ubuntu-latest
    steps:
    - name: Create issue using REST API
      run: |
        curl --request POST \
        --url https://api.github.com/repos/${{ github.repository }}/issues \
        --header 'authorization: Bearer ${{ secrets.GITHUB_TOKEN }}' \
        --header 'content-type: application/json' \
        --data '{
          "title": "Automated issue for commit: ${{ github.sha }}",
          "body": "This issue was automatically created by the GitHub Action workflow **${{ github.workflow }}**. \n\n The commit hash was: _${{ github.sha }}_."
          }'
```

##  `GITHUB_TOKEN`的权限

有关GitHub Apps可以使用每种权限访问的API端点的信息，请参阅“ [GitHub App权限”](https://docs.github.com/en/free-pro-team@latest/v3/apps/permissions)。

|   许可   | 访问类型 | 通过fork存储库访问 |
| :------: | :------: | :----------------: |
|   动作   |  读/写   |         读         |
|   检查   |  读/写   |         读         |
|   内容   |  读/写   |         读         |
|   部署   |  读/写   |         读         |
|   问题   |  读/写   |         读         |
|  元数据  |    读    |         读         |
|    包    |  读/写   |         读         |
| 拉取请求 |  读/写   |         读         |
| 仓库项目 |  读/写   |         读         |
|   状态   |  读/写   |         读         |

如果您需要令牌，而该令牌需要的权限不可用，则`GITHUB_TOKEN`可以创建个人访问令牌并将其设置为存储库中的机密：

1. 使用或创建对该存储库具有适当权限的令牌。有关更多信息，请参阅“[创建个人访问令牌”](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token)。
2. 将令牌作为机密添加到您的工作流存储库中，并使用`${{ secrets.SECRET_NAME }}`语法对其进行引用。有关更多信息，请参见“[创建和使用加密的机密”](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)。



## 参考阅读


