---
title: 启用调试日志记录
date: 2020-11-02 22:41:39
updated: 2020-11-02 22:41:39
mathjax: false
categories: 
tags:
typora-root-url: enabling-debug-logging
typora-copy-images-to: enabling-debug-logging
top: 1
comments: false
---


# 启用调试日志记录

[原文](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/enabling-debug-logging)

> 如果工作流日志没有提供足够的详细信息来诊断工作流，作业或步骤未按预期工作的原因，则可以启用其他调试日志记录。 



通过在包含工作流程的存储库中设置机密来启用这些额外的日志，因此将适用相同的权限要求：

* 要为用户帐户存储库创建机密，您必须是存储库所有者。要为组织存储库创建机密，您必须具有`admin`访问权限。
* 要在组织级别创建机密，您必须具有`admin`访问权限。
* 要使用REST API创建机密，您必须具有对存储库的写访问权限或对组织的管理员访问权限。有关更多信息，请参阅“ [GitHub Actions秘密API”](https://docs.github.com/en/free-pro-team@latest/v3/actions/secrets)。

有关设置机密的更多信息，请参阅“[创建和使用加密的机密”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)。

## 启用流道诊断日志记录

运行程序诊断日志记录提供了其他日志文件，其中包含有关运行程序如何执行作业的信息。将两个额外的日志文件添加到日志归档文件中：

* 运行程序进程日志，其中包括有关协调和设置运行程序以执行作业的运行程序的信息。
* 作业进程日志，用于记录作业的执行情况。

1. 要启用运行程序诊断日志，在包含工作流库中设置下列秘密：`ACTIONS_RUNNER_DEBUG`为`true`。
2. 要下载运行程序诊断日志，请下载工作流程运行的日志存档。运行程序诊断日志包含在该`runner-diagnostic-logs`文件夹中。有关下载日志的更多信息，请参阅“[下载日志”](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/using-workflow-run-logs/#downloading-logs)。

## 启用步骤调试日志记录

步骤调试日志记录增加了作业执行期间和执行之后作业日志的详细程度。

1. 要启用步骤调试日志记录，您必须在包含工作流库中设置下列秘密：`ACTIONS_STEP_DEBUG`为`true`。
2. 设置后，步骤日志中将显示更多调试事件。有关更多信息，请参阅[“查看日志以诊断故障”](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/using-workflow-run-logs/#viewing-logs-to-diagnose-failures)。



## 参考阅读


