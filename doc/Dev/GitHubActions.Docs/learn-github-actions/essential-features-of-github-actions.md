---
title: GitHub Actions的基本特征
date: 2020-10-31 13:44:07
updated: 2020-10-31 13:44:07
mathjax: false
categories: 
tags:
typora-root-url: essential-features-of-github-actions
typora-copy-images-to: essential-features-of-github-actions
top: 1
comments: false
---


# GitHub Actions的基本特征

[原文](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/essential-features-of-github-actions)

> GitHub Actions旨在帮助您构建健壮和动态的自动化。本指南将向您展示如何制作GitHub Actions工作流程，其中包括环境变量，自定义脚本等。 



## 总览

GitHub Actions允许您自定义工作流，以满足您的应用程序和团队的独特需求。在本指南中，我们将讨论一些基本的自定义技术，例如使用变量，运行脚本以及在作业之间共享数据和工件。

## 在工作流程中使用变量

GitHub Actions包含每个工作流程运行的默认环境变量。如果需要使用自定义环境变量，则可以在YAML工作流文件中进行设置。本示例演示了如何创建名为`POSTGRES_HOST`和`POSTGRES_PORT`的自定义变量。这些变量随后可用于`node client.js`脚本。

```yaml
jobs:
  example-job:
      steps:
        - name: Connect to PostgreSQL
          run: node client.js
          env:
            POSTGRES_HOST: postgres
            POSTGRES_PORT: 5432
```

有关更多信息，请参见“[使用环境变量”](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/using-environment-variables)。

## 将脚本添加到您的工作流程

您可以使用动作来运行脚本和Shell命令，然后在指定的运行程序上执行它们。此示例演示了动作如何使用`run`关键字在运行程序上执行`npm install -g bats`。

```yaml
jobs:
  example-job:
    steps:
      - run: npm install -g bats
```

例如，要将脚本作为动作运行，可以将脚本存储在存储库中，并提供路径和外壳程序类型。

```yaml
jobs:
  example-job:
    steps:
      - name: Run build script
        run: ./.github/scripts/build.sh
        shell: bash
```

有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsrun)”。

## 作业之间共享数据

如果您的作业生成了要与同一工作流中的其他工作共享的文件，或者想要保存文件以供以后参考，则可以将它们作为*工件*存储在GitHub中。工件是在构建和测试代码时创建的文件。例如，工件可能包括二进制文件或软件包文件，测试结果，屏幕截图或日志文件。工件与创建它们的工作流运行相关联，并且可以由其他作业使用。

例如，您可以创建一个文件，然后将其作为工件上传。

```yaml
jobs:
  example-job:
    name: Save output
    steps:
      - shell: bash
        run: |
          expr 1 + 1 > output.log
      - name: Upload output file
        uses: actions/upload-artifact@v1
        with:
          name: output-log-file
          path: output.log
```

要从单独的工作流程运行中下载工件，可以使用`actions/download-artifact`动作。例如，您可以下载名为`output-log-file`的工件。

```yaml
jobs:
  example-job:
    steps:
      - name: Download a single artifact
        uses: actions/download-artifact@v2
        with:
          name: output-log-file
```

有关工件的更多信息，请参见“[使用工件持久化工作流数据](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/persisting-workflow-data-using-artifacts)”。

## 下一步

要继续学习GitHub Action，请参阅“[管理复杂的工作流程”](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/managing-complex-workflows)。



## 参考阅读


