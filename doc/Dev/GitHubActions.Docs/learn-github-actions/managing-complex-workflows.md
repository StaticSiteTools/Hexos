---
title: 管理复杂的工作流程
date: 2020-10-31 13:54:56
updated: 2020-10-31 13:54:56
mathjax: false
categories: 
tags:
typora-root-url: managing-complex-workflows
typora-copy-images-to: managing-complex-workflows
top: 1
comments: false
---


# 管理复杂的工作流程

[原文](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/managing-complex-workflows)

> 本指南向您展示了如何使用GitHub Actions的高级功能以及机密管理，相关作业，缓存，构建矩阵和标签。 



## 储存机密

如果您的工作流使用敏感数据，例如密码或证书，则可以将它们作为*机密*保存在GitHub中，然后在工作流中将它们用作环境变量。这意味着您将能够创建和共享工作流程，而不必直接在YAML工作流程中嵌入敏感值。

此示例动作演示了如何将现有机密作为环境变量引用，并将其作为参数发送到示例命令。

```yaml
jobs:
  example-job:
    steps:
      - name: Retrieve secret
        env:
          super_secret: ${{ secrets.SUPERSECRET }}
        run: |
          example-command "$super_secret"
```

有关更多信息，请参见“[创建和存储加密的机密”](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)。

## 创建依赖作业

默认情况下，工作流中的所有作业都同时并行运行。因此，如果您有一个只能在另一个作业完成后才能运行的作业，则可以使用`needs`关键字创建此依赖关系。如果其中一项作业失败，则将跳过所有从属作业；但是，如果需要继续作业，则可以使用[`if`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idif)条件语句进行定义。

在这个例子中，`setup`，`build`，和`test`作业以串联方式运行 ，`build`和`test`依赖于成功完成之前的作业 ：

```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
      - run: ./setup_server.sh
  build:
    needs: setup
    steps:
      - run: ./build_server.sh
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: ./test_server.sh 
```

有关更多信息，请参见[`jobs.<job_id>.needs`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idneeds)。

## 使用构建矩阵

如果希望工作流跨操作系统，平台和语言的多种组合运行测试，则可以使用构建矩阵。使用`strategy`关键字创建构建矩阵，该关键字以数组形式接收构建选项。例如，此构建矩阵将使用不同版本的Node.js多次运行该作业：

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [6, 8, 10]
    steps:
      - uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node }}
```

有关更多信息，请参见[`obs.<job_id>.strategy.matrix`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix)。

## 缓存依赖

GitHub托管的运行程序是为每个作业的全新环境启动的，因此，如果您的作业定期重复使用依赖项，则可以考虑将这些文件缓存以帮助提高性能。创建缓存后，该缓存可用于同一存储库中的所有工作流程。

本示例演示如何缓存` ~/.npm`目录：

```yaml
jobs:
  example-job:
    steps:
      - name: Cache node modules
        uses: actions/cache@v2
        env:
          cache-name: cache-node-modules
        with:
          path: ~/.npm
          key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-build-${{ env.cache-name }}-
```

有关更多信息，请参阅“[缓存依赖项以加快工作流程”](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/caching-dependencies-to-speed-up-workflows)。

## 使用数据库和服务容器

如果您的工作需要数据库或缓存服务，则可以使用[`services`](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#jobsjob_idservices)关键字创建一个临时容器来承载该服务；然后，生成的容器可用于该作业中的所有步骤，并在作业完成后将其删除。此示例演示了作业如何用`services`创建`postgres`容器，然后用`node`连接到服务。

```yaml
jobs:
  container-job:
    runs-on: ubuntu-latest
    container: node:10.18-jessie
    services:
      postgres:
        image: postgres
    steps:
      - name: Check out repository code
        uses: actions/checkout@v2
      - name: Install dependencies
        run: npm ci
      - name: Connect to PostgreSQL
        run: node client.js
        env:
          POSTGRES_HOST: postgres
          POSTGRES_PORT: 5432
```

有关更多信息，请参见“[使用数据库和服务容器”](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/using-databases-and-service-containers)。

## 使用标签路由工作流程

此功能可帮助您将作业分配给特定的自托管运行器。如果要确保特定类型的运行程序将处理您的作业，可以使用标签来控制作业的执行位置。您可以将标签指定给自托管运行器，然后在YAML工作流中引用这些标签，以确保作业以可预测的方式路由。

此示例显示工作流如何使用标签来指定所需的运行器：

```yaml
jobs:
  example-job:
      runs-on: [self-hosted, linux, x64, gpu]
```

有关更多信息，请参阅 [“将标签与自承载流道一起使用](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/using-labels-with-self-hosted-runners)”。

## 下一步

要继续了解GitHub Action，请参阅“[与组织共享工作流](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/sharing-workflows-with-your-organization)”。





## 参考阅读


