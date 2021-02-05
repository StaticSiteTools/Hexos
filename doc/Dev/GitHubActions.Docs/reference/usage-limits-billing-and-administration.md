---
title: 使用限制，计费和管理
date: 2020-11-10 21:37:17
updated: 2020-11-10 21:37:17
mathjax: false
categories: 
tags:
typora-root-url: usage-limits-billing-and-administration
typora-copy-images-to: usage-limits-billing-and-administration
top: 1
comments: false
---


# 使用限制，计费和管理

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/usage-limits-billing-and-administration)

> GitHub Actions工作流程有使用限制。使用费适用于超出免费时间和存储库数量的存储库。 



## 关于GitHub动作的计费

公共存储库和自托管运行者免费使用GitHub Actions。对于私有存储库，每个GitHub帐户都会获得一定数量的空闲时间和存储空间，具体取决于该帐户使用的产品。有关更多信息，请参阅“[关于GitHub动作的计费](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions)”。

## 使用限制

使用GitHub托管的运行程序时，对GitHub Actions的使用有一些限制。这些限制可能会更改。

**注意：**对于自托管的运行者，适用不同的使用限制。有关更多信息，请参见“[关于自托管运行者”](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners/#usage-limits)。

* **作业执行时间**-工作流中的每个作业最多可以运行6个小时的执行时间。如果作业达到此限制，则作业将终止并且无法完成。

* **工作流程运行时间**-每个工作流程运行限制为72小时。如果工作流程运行达到此限制，则取消工作流程运行。

* **API请求**-您在一个存储库中的所有动作中，一个小时内最多可以执行1000个API请求。如果超过该数量，则其他API调用将失败，这可能导致作业失败。

* **并发作业**-可以在您的帐户中运行的并发作业数取决于您的GitHub计划，如下表所示。如果超过，则将对其他作业进行排队。

  | GitHub计划 | 并发工作总数 | 最大并发macOS作业 |
  | ---------- | ------------ | ----------------- |
  | 自由       | 20           | 5                 |
  | 专业版     | 40           | 5                 |
  | 团队       | 60           | 5                 |
  | 企业       | 180          | 50                |

* **作业矩阵**- 每个工作流程运行最多可生成256个作业。此限制也适用于自托管运行者。

## 使用政策

除了使用限制外，您还必须确保在[GitHub服务条款中](https://docs.github.com/en/free-pro-team@latest/articles/github-terms-of-service)使用GitHub Actions 。有关特定于GitHub Actions术语的更多信息，请参阅[GitHub Additional Product Terms](https://docs.github.com/en/free-pro-team@latest/github/site-policy/github-additional-product-terms#a-actions-usage)。

## 工件和日志保留策略

您可以为存储库，组织或企业帐户配置工件和日志保留期。

默认情况下，工作流生成的工件和日志文件会保留90天，然后自动删除。您可以根据存储库的类型调整保留期：

* 对于公共存储库：您可以将此保留期限更改为1天或90天之间的任意时间。
* 对于私有，内部和GitHub 企业存储库：您可以将此保留期限更改为1天或400天之间的任意时间。

自定义保留期时，它仅适用于新的工件和日志文件，而不能追溯适用于现有对象。对于托管存储库和组织，最大保留期限不能超过管理组织或企业设置的限制。

有关更多信息，请参见：

* [为存储库中的工件和日志配置GitHub Actions的保留期](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/configuring-the-retention-period-for-github-actions-artifacts-and-logs-in-your-repository)
* [为组织中的工件和日志配置GitHub Actions的保留期](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-organizations-and-teams/configuring-the-retention-period-for-github-actions-artifacts-and-logs-in-your-organization)
* [为企业中的工件和日志配置GitHub Actions的保留期](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-your-enterprise/configuring-the-retention-period-for-github-actions-artifacts-and-logs-in-your-enterprise-account)

## 为您的存储库或组织禁用或限制GitHub动作

默认情况下，所有存储库和组织上都启用了GitHub Actions。您可以选择禁用GitHub Actions或将其限制为私有动作，这意味着人们只能使用存储库中存在的动作。

有关更多信息，请参见：

* “[为存储库禁用或限制GitHub动作](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/disabling-or-limiting-github-actions-for-a-repository)”
* “[为您的组织禁用或限制GitHub动作](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-organizations-and-teams/disabling-or-limiting-github-actions-for-your-organization)”
* 针对GitHub企业云的“[在企业帐户中实施GitHub动作策略](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-your-enterprise/enforcing-github-actions-policies-in-your-enterprise-account)”

## 禁用和启用工作流程

您可以在GitHub上的存储库中启用和禁用单个工作流程。

为了防止不必要的工作流程运行，计划的工作流程可能会自动禁用。派生公共存储库时，默认情况下禁用计划的工作流程。在公共存储库中，如果60天内未发生任何存储库活动，则计划的工作流将自动禁用。

有关更多信息，请参阅“[禁用和启用工作流程”](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/disabling-and-enabling-a-workflow)。



## 参考阅读


