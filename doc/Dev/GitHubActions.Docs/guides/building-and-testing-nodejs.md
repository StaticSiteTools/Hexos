---
title: 构建和测试Node.js
date: 2020-11-02 22:55:53
updated: 2020-11-02 22:55:53
mathjax: false
categories: 
tags:
typora-root-url: building-and-testing-nodejs
typora-copy-images-to: building-and-testing-nodejs
top: 1
comments: false
---


# 构建和测试Node.js

[原文](https://docs.github.com/en/free-pro-team@latest/actions/guides/building-and-testing-nodejs)

> 您可以创建一个持续集成（CI）工作流来构建和测试您的Node.js项目。 



## 介绍

本指南向您展示如何创建用于构建和测试Node.js代码的持续集成（CI）工作流。如果您的CI测试通过，则可能要部署代码或发布程序包。

## 先决条件

我们建议您对Node.js，YAML，工作流配置选项以及如何创建工作流文件有基本的了解。有关更多信息，请参见：

* “[学习GitHub动作](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions)”
* “ [Node.js入门](https://nodejs.org/en/docs/guides/getting-started-guide/)”



## 从Node.js工作流程模板开始

GitHub提供了适用于大多数Node.js项目的Node.js工作流模板。本指南包含可用于自定义模板的npm和Yarn示例。有关更多信息，请参见[Node.js工作流模板](https://github.com/actions/starter-workflows/blob/main/ci/node.js.yml)。

为了快速入门，请将模板添加到存储库的`.github/workflows`目录中。

```yaml
name: Node.js CI

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [8.x, 10.x, 12.x]

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm install
    - run: npm run build --if-present
    - run: npm test
      env:
        CI: true
```



## 在其他操作系统上运行

入门工作流程模板使用GitHub托管的运行程序将作业配置为在Linux上运行`ubuntu-latest`。您可以更改`runs-on`键以在其他动作系统上运行作业。例如，您可以使用GitHub托管的Windows运行程序。

```
runs-on: windows-latest
```

或者，您可以在GitHub托管的macOS运行程序上运行。

```
runs-on: macos-latest
```

您还可以在Docker容器中运行作业，或者可以提供在您自己的基础架构上运行的自托管运行器。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idruns-on)”。



## 指定Node.js版本

指定Node.js版本的最简单方法是使用GitHub提供的动作`setup-node`。有关更多信息，请参见[`setup-node`](https://github.com/actions/setup-node/)。

该`setup-node`动作将Node.js版本作为输入，并在运行程序上配置该版本。该`setup-node`动作从每个运行器的工具缓存中找到特定版本的Node.js，并将必需的二进制文件添加到中`PATH`，该二进制文件将在其余作业中保持不变。在GitHub动作中使用`setup-node`动作是推荐的方法，因为它可以确保不同运行程序和不同版本的Node.js的行为一致。如果您使用的是自托管运行器，则必须安装Node.js并将其添加到`PATH`中。

该模板包括一个矩阵策略，该策略使用三种Node.js版本（8.x，10.x和12.x）来构建和测试代码。“ x”是通配符，与某个版本可用的最新次要版本和修补程序版本匹配。数组中`node-version`指定的每个版本的Node.js都会创建一个运行相同步骤的作业。

每个作业都可以使用`matrix`上下文访问矩阵`node-version`数组中定义的值。`setup-node`动作将上下文用作`node-version`输入。`setup-node`在构建和测试代码之前，该动作将为每个作业配置不同的Node.js版本。有关矩阵策略和上下文的更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix)”和“ [GitHub Actions的](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix)[上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)”。

```yaml
strategy:
  matrix:
    node-version: [8.x, 10.x, 12.x]

steps:
- uses: actions/checkout@v2
- name: Use Node.js ${{ matrix.node-version }}
  uses: actions/setup-node@v1
  with:
    node-version: ${{ matrix.node-version }}
```

或者，您可以使用确切的Node.js版本进行构建和测试。

```yaml
strategy:
  matrix:
    node-version: [8.16.2, 10.17.0]
```

或者，您也可以使用单个版本的Node.js进行构建和测试。

```yaml
name: Node.js CI

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'
    - run: npm install
    - run: npm run build --if-present
    - run: npm test
      env:
        CI: true
```

如果您未指定Node.js版本，则GitHub使用环境的默认Node.js版本。有关更多信息，请参见“ [GitHub托管的运行程序规范](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners/#supported-software)”。

## 安装依赖

GitHub托管的运行程序安装了npm和Yarn依赖管理器。在构建和测试代码之前，可以使用npm和Yarn在工作流中安装依赖项。Windows和Linux GitHub托管的运行程序还安装了Grunt，Gulp和Bower。

您还可以缓存依赖项以加快工作流程。有关更多信息，请参阅“[缓存依赖项以加快工作流程”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/caching-dependencies-to-speed-up-workflows)。

### 使用npm的示例

本示例将安装*package.json*文件中定义的依赖项。有关更多信息，请参见[`npm install`](https://docs.npmjs.com/cli/install)。

```yaml
steps:
- uses: actions/checkout@v2
- name: Use Node.js
  uses: actions/setup-node@v1
  with:
    node-version: '12.x'
- name: Install dependencies
  run: npm install
```

使用`npm ci`将安装在*package-lock.json*或*npm* *-shrinkwrap.json*文件中的版本，并防止更新锁文件。使用`npm ci`速度通常比运行`npm install`速度快。有关更多信息，请参见[`npm ci`](https://docs.npmjs.com/cli/ci.html)和“[介绍更快，更可靠的构建`npm ci`](https://blog.npmjs.org/post/171556855892/introducing-npm-ci-for-faster-more-reliable)。”

```yaml
steps:
- uses: actions/checkout@v2
- name: Use Node.js
  uses: actions/setup-node@v1
  with:
    node-version: '12.x'
- name: Install dependencies
  run: npm ci
```

### 使用Yarn的例子

本示例将安装*package.json*文件中定义的依赖项。有关更多信息，请参见[`yarn install`](https://yarnpkg.com/en/docs/cli/install)。

```
steps:
- uses: actions/checkout@v2
- name: Use Node.js
  uses: actions/setup-node@v1
  with:
    node-version: '12.x'
- name: Install dependencies
  run: yarn
```

或者，您可以通过`--frozen-lockfile`将版本安装在*yarn.lock*文件中，并防止更新*yarn.lock*文件。

```yaml
steps:
- uses: actions/checkout@v2
- name: Use Node.js
  uses: actions/setup-node@v1
  with:
    node-version: '12.x'
- name: Install dependencies
  run: yarn --frozen-lockfile
```

### 使用私有注册表并创建.npmrc文件的示例

您可以使用该`setup-node`动作在运行程序上创建本地`.npmrc`文件，以配置默认注册表和范围。该`setup-node`动作还接受身份验证令牌作为输入，用于访问私有注册表或发布Node程序包。有关更多信息，请参见[`setup-node`](https://github.com/actions/setup-node/)。

要对您的私有注册表进行身份验证，您需要将npm身份验证令牌作为密钥存储在存储库设置中。例如，创建一个名为`NPM_TOKEN`的秘密。有关更多信息，请参见“[创建和使用加密的机密”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)。

在下面的示例中，密钥`NPM_TOKEN`存储npm身份验证令牌。该`setup-node`动作将*.npmrc*文件配置为从环境变量读取npm身份验证令牌`NODE_AUTH_TOKEN`。使用`setup-node`动作创建*.npmrc*文件时，必须使用包含npm身份验证令牌的密钥来设置`NPM_AUTH_TOKEN`环境变量。

在安装依赖项之前，请使用该`setup-node`动作创建*.npmrc*文件。该动作有两个输入参数。该`node-version`参数设置Node.js版本，该`registry-url`参数设置默认注册表。如果程序包注册表使用范围，则必须使用`scope`参数。有关更多信息，请参见[`npm-scope`](https://docs.npmjs.com/misc/scope)。

```yaml
steps:
- uses: actions/checkout@v2
- name: Use Node.js
  uses: actions/setup-node@v1
  with:
    always-auth: true
    node-version: '12.x'
    registry-url: https://registry.npmjs.org
    scope: '@octocat'
- name: Install dependencies
  run: npm ci
  env:
    NODE_AUTH_TOKEN: ${{secrets.NPM_TOKEN}}
```

上面的示例创建一个*.npmrc*文件，其内容如下：

```
//registry.npmjs.org/:_authToken=${NODE_AUTH_TOKEN}
@octocat:registry=https://registry.npmjs.org/
always-auth=true
```

### 缓存依赖示例

您可以使用唯一键缓存依赖关系，并在使用`cache`动作运行将来的工作流时还原依赖关系。有关更多信息，请参阅“[缓存依赖项以加快工作流程](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/caching-dependencies-to-speed-up-workflows)”和[`cache`动作](https://github.com/marketplace/actions/cache)。

```yaml
steps:
- uses: actions/checkout@v2
- name: Use Node.js
  uses: actions/setup-node@v1
  with:
    node-version: '12.x'
- name: Cache Node.js modules
  uses: actions/cache@v2
  with:
    # npm cache files are stored in `~/.npm` on Linux/macOS
    path: ~/.npm 
    key: ${{ runner.OS }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.OS }}-node-
      ${{ runner.OS }}-
- name: Install dependencies
  run: npm ci
```

## 构建和测试您的代码

您可以使用本地使用的相同命令来构建和测试代码。例如，如果您运行`npm run build`来运行*package.json*文件中定义的构建步骤并且运行`npm test`来测试套件，则可以将这些命令添加到工作流文件中。

```yaml
steps:
- uses: actions/checkout@v2
- name: Use Node.js
  uses: actions/setup-node@v1
  with:
    node-version: '12.x'
- run: npm install
- run: npm run build --if-present
- run: npm test
```

## 将工作流数据打包为工件

您可以从构建和测试步骤中保存工件，以在作业完成后进行查看。例如，您可能需要保存日志文件，核心转储，测试结果或屏幕截图。有关更多信息，请参见“[使用构件持久化工作流数据](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/persisting-workflow-data-using-artifacts)”。

## 发布到程序包注册表

您可以配置工作流程以在CI测试通过后将Node.js程序包发布到程序包注册表。有关发布到npm和GitHub软件包的更多信息，请参见“[发布Node.js软件包”](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/publishing-nodejs-packages)。





## 参考阅读


