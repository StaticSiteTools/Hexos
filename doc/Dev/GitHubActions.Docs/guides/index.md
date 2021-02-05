---
title: 指南
date: 2020-11-03  7:12:08
updated: 2020-11-03  7:12:08
mathjax: false
categories: 
tags:
typora-root-url: index
typora-copy-images-to: index
top: 1
comments: false
---


# 指南

[原文](https://docs.github.com/en/free-pro-team@latest/actions/guides)

> 这些GitHub Actions指南包括特定的用例和示例，以帮助您配置工作流程。 



## 创建自定义的持续集成工作流程

您可以使用GitHub Actions创建自定义的持续集成（CI）工作流，以构建和测试以不同编程语言编写的项目。

* [关于持续集成](https://docs.github.com/en/free-pro-team@latest/actions/guides/about-continuous-integration)  [md](about-continuous-integration.md)
* [使用工作流程模板设置持续集成](https://docs.github.com/en/free-pro-team@latest/actions/guides/setting-up-continuous-integration-using-workflow-templates)  [md](setting-up-continuous-integration-using-workflow-templates.md)
* [构建和测试Node.js](https://docs.github.com/en/free-pro-team@latest/actions/guides/building-and-testing-nodejs)   [md](building-and-testing-nodejs.md)
* [构建和测试Python](https://docs.github.com/en/free-pro-team@latest/actions/guides/building-and-testing-python)
* [使用Maven构建和测试Java](https://docs.github.com/en/free-pro-team@latest/actions/guides/building-and-testing-java-with-maven)
* [使用Gradle构建和测试Java](https://docs.github.com/en/free-pro-team@latest/actions/guides/building-and-testing-java-with-gradle)
* [使用Ant构建和测试Java](https://docs.github.com/en/free-pro-team@latest/actions/guides/building-and-testing-java-with-ant)

## 发布软件包

您可以将发布软件包自动化，作为持续交付（CD）工作流程的一部分。软件包可以发布到任何软件包主机和GitHub上。软件包可通过GitHub Free，GitHub Pro，适用于组织的GitHub Free，GitHub Team，GitHub Enterprise Cloud，GitHub Enterprise Server 2.22和GitHub One获得。

GitHub软件包不适用于使用旧版每个存储库计划的帐户拥有的私有存储库。另外，使用旧的每个存储库计划的帐户无法访问GitHub Container Registry，因为这些帐户是按存储库计费的。有关更多信息，请参见“ [GitHub的产品”](https://docs.github.com/en/free-pro-team@latest/articles/github-s-products)。。

* [关于使用GitHub Actions打包](https://docs.github.com/en/free-pro-team@latest/actions/guides/about-packaging-with-github-actions)
* [发布Node.js包](https://docs.github.com/en/free-pro-team@latest/actions/guides/publishing-nodejs-packages)
* [使用Maven发布Java软件包](https://docs.github.com/en/free-pro-team@latest/actions/guides/publishing-java-packages-with-maven)
* [用Gradle发布Java包](https://docs.github.com/en/free-pro-team@latest/actions/guides/publishing-java-packages-with-gradle)
* [发布Docker映像](https://docs.github.com/en/free-pro-team@latest/actions/guides/publishing-docker-images)

## 缓存和存储工作流数据

缓存依赖项并存储工件，以使您的工作流运行更有效率。

* [将工作流数据存储为工件](https://docs.github.com/en/free-pro-team@latest/actions/guides/storing-workflow-data-as-artifacts)
* [缓存依赖项以加快工作流程](https://docs.github.com/en/free-pro-team@latest/actions/guides/caching-dependencies-to-speed-up-workflows)

## 在工作流程中使用服务容器

使用服务容器将服务连接到您的工作流。

* [关于服务容器](https://docs.github.com/en/free-pro-team@latest/actions/guides/about-service-containers)
* [创建Redis服务容器](https://docs.github.com/en/free-pro-team@latest/actions/guides/creating-redis-service-containers)
* [创建PostgreSQL服务容器](https://docs.github.com/en/free-pro-team@latest/actions/guides/creating-postgresql-service-containers)



## 参考阅读


