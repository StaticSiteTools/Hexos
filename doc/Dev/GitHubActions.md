---
title: GitHub Actions
date: 2020-11-10 22:35:36
updated: 2020-11-10 22:35:36
mathjax: false
categories: 
tags:
typora-root-url: GitHubActions
typora-copy-images-to: GitHubActions
top: 1
comments: false
---


# GitHub Actions

[原文](https://docs.github.com/en/free-pro-team@latest/actions)



## 序言

> 本文为摘录官方文档的部分内容，详细的请参阅  [官方文档](https://docs.github.com/en/free-pro-team@latest/actions)



## 概览

**GitHub Actions** ：  GitHub 的持续集成服务，于2018年10月推出。

使用GitHub Actions在存储库中自动化，自定义和执行软件开发工作流程。您可以发现，创建和共享动作以执行所需的任何作业（包括CI / CD），并在完全定制的工作流程中组合动作。 

大家知道，持续集成由很多操作组成，比如抓取代码、运行测试、登录远程服务器，发布到第三方服务等等。GitHub 把这些操作就称为 actions。[^1] 

GitHub 做了一个[官方市场](https://github.com/marketplace?type=actions)，可以搜索到他人提交的 actions。另外，还有一个 [awesome actions](https://github.com/sdras/awesome-actions) 的仓库，也可以找到不少 action。 [^1]

GitHub Actions可帮助您在软件开发生命周期内自动化任务。GitHub Actions是事件驱动的，这意味着您可以在发生指定事件后运行一系列命令。例如，每当有人为存储库创建拉取请求时，您可以自动运行执行软件测试脚本的命令。



## GitHub Actions的组件

> 参阅：[GitHub Actions简介](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)   [md](GitHubActions.Docs/learn-github-actions/introduction-to-github-actions.md)

下面是多个协同运行作业的GitHub Actions组件的列表。您可以看到这些组件是如何相互作用的。

![组件和服务概述](overview-actions-design.png)



### 工作流程(Workflows)

工作流程是您添加到存储库中的自动化过程。工作流由一个或多个作业组成，可以由事件安排或触发。该工作流程可用于在GitHub上构建，测试，打包，发布或部署项目。

### 事件(Events)

事件是触发工作流程的特定活动。例如，当有人将提交推送到存储库或创建问题或请求请求时，活动可以源自GitHub。您还可以使用存储库调度Webhook在发生外部事件时触发工作流。有关可用于触发工作流的事件的完整列表，请参阅《[触发工作流的事件》](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)。

### 作业(Jobs)

作业是在同一运行程序上执行的一组步骤。默认情况下，具有多个作业的工作流程将并行运行这些作业。您还可以配置工作流以按顺序运行作业。例如，一个工作流可以有两个顺序的作业来构建和测试代码，其中测试作业取决于构建作业的状态。如果构建作业失败，则测试作业将不会运行。

### 步骤(Steps)

步骤是可以运行命令的单个任务（称为*action*）。作业中的每个步骤都在同一运行程序上执行，从而使该作业中的动作可以彼此共享数据。

### 动作(Actions)

动作是独立的命令，它们被组合成步骤来创建作业。 动作是工作流中最小的可移植构建块。您可以创建自己的动作，也可以使用GitHub社区创建的动作。要在工作流中使用动作，您必须将其包括为一个步骤。

### 运行者(Runners)

运行程序是已安装GitHub Actions运行程序应用程序的服务器。您可以使用GitHub托管的运行程序，也可以托管自己的运行程序。运行程序侦听可用的作业，一次运行一个作业，并将进度，日志和结果报告回GitHub。对于由GitHub托管的运行程序，工作流程中的每个**作业**都在全新的虚拟环境中运行。

GitHub托管的运行程序基于Ubuntu Linux，Microsoft Windows和macOS。有关GitHub托管的运行程序的信息，请参阅“ [GitHub托管的运行程序的虚拟环境](https://docs.github.com/en/free-pro-team@latest/actions/reference/virtual-environments-for-github-hosted-runners)”。如果您需要其他操作系统或需要特定的硬件配置，则可以托管自己的运行程序。有关自托管运行程序的信息，请参阅“[托管您自己的运行程序”](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners)。



## 使用指南

### 创建示例工作流程

> 参阅： [GitHub Actions简介](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)   [md](GitHubActions.Docs/learn-github-actions/introduction-to-github-actions.md)

GitHub Actions使用YAML语法定义事件，作业和步骤。这些YAML文件存储在您的代码存储库中的`.github/workflows`目录中。

您可以在存储库中创建一个示例工作流，每当推送代码时，它都会自动触发一系列命令。 在此工作流程中，GitHub Actions签出推送的代码，安装软件依赖项并运行`bats -v`。

1. 在您的存储库中，创建`.github/workflows/`目录以存储您的工作流文件。

2. 在 `.github/workflows/`  目录中，创建一个名为 `learn-github-actions.yml`  的新文件，并添加以下代码。

   ```yaml
   name: learn-github-actions
   on: [push]
   jobs:
     check-bats-version:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - uses: actions/setup-node@v1
         - run: npm install -g bats
         - run: bats -v
   ```

3. 提交这些更改并将其推送到您的GitHub存储库。

现在，您的新GitHub Actions工作流文件已安装在您的存储库中，并且每次有人将更改推送到存储库时将自动运行。有关作业执行历史的详细信息，请参阅“[查看工作流程的活动”](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions#viewing-the-jobs-activity)。

### 了解工作流程文件

> 参阅： [GitHub Actions简介](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)   [md](GitHubActions.Docs/learn-github-actions/introduction-to-github-actions.md)

为了帮助您了解如何使用YAML语法创建工作流文件，本节说明了简介示例的每一行：

| 代码                                | 说明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| `name: learn-github-actions`        | *可选*-工作流的名称，它将显示在GitHub存储库的“动作”选项卡中。 |
| `on: [push]`                        | 指定自动触发工作流文件的事件。本示例使用`push`事件，以便每当有人将更改推送到存储库时作业就运行。您可以将工作流设置为仅在某些分支，路径或标签上运行。有关包括或不包括分支，路径或标签的语法示例，请参阅[“ GitHub Actions的工作流语法”。](https://docs.github.com/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestpaths) |
| `jobs:`                             | 将`learn-github-actions`工作流文件中运行的所有作业分组在一起。 |
| `check-bats-version:`               | 定义存储在`jobs`部分中的`check-bats-version`作业的名称。     |
| `  runs-on: ubuntu-latest`          | 配置作业以在Ubuntu Linux运行器上运行。这意味着该作业将在GitHub托管的新虚拟机上执行。有关使用其他运行程序的语法示例，请参阅[“ GitHub Actions的工作流语法”。](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) |
| `  steps:`                          | 将`check-bats-version`作业中运行的所有步骤组合在一起。嵌套在此部分下的每一行都是单独的动作。 |
| `    - uses: actions/checkout@v2`   | `uses`关键字告诉作业检索名为`actions/checkout@v2`的社区动作的`v2`。此动作将检出您的存储库并将其下载到运行程序，从而使您可以对代码（例如测试工具）运行动作。每当工作流将根据存储库的代码运行时，或者使用存储库中定义的动作时，都必须使用checkout动作。 |
| `    - uses: actions/setup-node@v1` | 该动作将`node`软件包安装在运行程序上，从而使您可以访问`npm`命令。 |
| `    - run: npm install -g bats`    | `run`关键字告诉作业在运行程序上执行命令。在本例中，您使用的是`npm`来安装`bats`软件测试包。 |
| `    - run: bats -v`                | 最后，您将使用参数运行`bats`命令输出软件版本                 |

**可视化工作流程文件**

在此图中，您可以看到刚刚创建的工作流文件以及GitHub Actions组件在层次结构中的组织方式。每个步骤执行一个动作。步骤1和2使用预先构建的社区动作。要为您的工作流查找更多的预建动作，请参阅“[查找和自定义动作”](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions)。

![工作流程概述](/overview-actions-event.png)



### 查看工作流结果

> 参阅： [GitHub动作快速入门](https://docs.github.com/en/free-pro-team@latest/actions/quickstart)    [md](GitHubActions.Docs/quickstart.md)

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![img](/actions-tab.png) 

3. 在左侧边栏中，单击要查看的工作流。 

   ![img](/superlinter-workflow-sidebar.png) 

4. 从工作流运行的列表中，单击要查看的运行的名称。 

   ![img](/superlinter-run-name.png) 

5. 在左侧边栏中，单击**Lint code base**作业(任务)。

    ![img](/superlinter-lint-code-base-job.png) 

6. 任何失败的步骤都会自动展开以显示结果。 

   ![img](/super-linter-workflow-results-updated.png) 



### 在工作流程中使用变量

> 参阅：
>
> [GitHub Actions的基本特征](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/essential-features-of-github-actions)  [md](GitHubActions.Docs/learn-github-actions/essential-features-of-github-actions.md)
>
> [环境变量](https://docs.github.com/en/free-pro-team@latest/actions/reference/environment-variables)   [md](GitHubActions.Docs/reference/environment-variables.md)

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

#### 关于环境变量

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

#### 默认环境变量

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

#### 环境变量的命名约定

> **注意：** GitHub保留环境变量前缀`GITHUB_`供GitHub内部使用。使用`GITHUB_`前缀设置环境变量或密钥将导致错误。

您设置的指向文件系统上某个位置的任何新环境变量都应带有`_PATH`后缀。`HOME`和`GITHUB_WORKSPACE`缺省变量是例外约定，因为字“home”和“workspace”已暗示的位置。



### 将脚本添加到您的工作流程

> 参阅：[GitHub Actions的基本特征](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/essential-features-of-github-actions)  [md](GitHubActions.Docs/learn-github-actions/essential-features-of-github-actions.md)

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



### 储存机密

> 参阅：
>
> [管理复杂的工作流程](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/managing-complex-workflows)  [md](GitHubActions.Docs/learn-github-actions/managing-complex-workflows.md)
>
> [加密机密](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets)     [md](GitHubActions.Docs/reference/encrypted-secrets.md)

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

#### 关于机密

机密是您在存储库或组织中创建的加密环境变量。您创建的机密可在GitHub Actions工作流程中使用。GitHub使用[libsodium密封盒](https://libsodium.gitbook.io/doc/public-key_cryptography/sealed_boxes)来帮助确保机密在到达GitHub之前已加密，并且在您在工作流程中使用它们之前一直保持加密状态。

对于存储在组织级别的机密，您可以使用访问策略来控制哪些存储库可以使用组织机密。组织级机密使您可以在多个存储库之间共享机密，从而减少了创建重复机密的需要。在一个位置更新组织机密还可以确保更改在使用该机密的所有存储库工作流程中生效。

##### 命名您的机密

以下规则适用于机密名称：

* 机密名称只能包含字母数字字符（`[a-z]`，`[A-Z]`，`[0-9]`）或下划线（`_`）。不允许使用空格。
* 机密名称不能以`GITHUB_`前缀开头。
* 机密名称不能以数字开头。
* 机密名称在创建它们的级别上必须是唯一的。例如，在组织级别创建的机密必须在该级别具有唯一的名称，而在存储库级别创建的机密在该存储库中必须具有唯一的名称。如果组织级别的机密与存储库级的机密名称相同，则存储库级的机密优先。

为了帮助确保GitHub修改日志中的机密信息，请避免将结构化数据用作机密信息的值。例如，避免创建包含JSON或编码的Git Blob的机密。

##### 访问您的机密

要使机密可用于动作，必须在工作流文件中将机密设置为输入或环境变量。查看动作的README文件，以了解动作预期的输入和环境变量。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions/#jobsjob_idstepsenv)”。

如果您有权编辑文件，则可以使用和读取工作流文件中的加密机密。有关更多信息，请参阅“ [GitHub上的访问权限](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/access-permissions-on-github)”。

> **警告：** GitHub会自动编辑打印到日志的机密，但您应避免有意将机密打印到日志。

您还可以使用REST API管理机密。有关更多信息，请参见“[机密”](https://docs.github.com/en/free-pro-team@latest/v3/actions/secrets)。

##### 限制凭证权限

生成凭据时，建议您授予尽可能少的权限。例如，不要使用个人凭据，而要使用[部署密钥](https://docs.github.com/en/free-pro-team@latest/v3/guides/managing-deploy-keys/#deploy-keys)或服务帐户。如果需要，请考虑授予只读权限，并尽可能限制访问。生成个人访问令牌（PAT）时，请选择最少的范围。



#### 为存储库创建加密的机密

要为用户帐户存储库创建机密，您必须是存储库所有者。要为组织存储库创建机密，您必须具有`admin`访问权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击 **设置**。

   ![img](/repo-actions-settings.png) 

3. 在左侧边栏中，点击**机密(Secrets)**。

4. 单击**添加新机密**。

5. 在**名称**输入框中输入您的机密**名称**。

6. 输入您的机密值。

7. 单击**添加机密**。

如果您的存储库可以访问上级组织的机密，那么这些机密也会在此页面上列出。 



#### 为组织创建加密的机密

在组织中创建机密时，可以使用策略来限制哪些存储库可以访问该机密。例如，您可以授予对所有存储库的访问权限，或者将访问权限限制为仅私有存储库或指定的存储库列表。

要在组织级别创建机密，您必须具有`admin`访问权限。

1. 在GitHub上，导航到组织的主页。

2. 在您的组织名称下，点击 **设置**。

   ![img](/organization-settings-tab.png) 

3. 在左侧边栏中，点击**机密(Secrets)**。

4. 单击“**新机密”**。

5. 在**名称**输入框中输入您的机密**名称**。

6. 输入您的机密**值**。

7. 从“**存储库访问”**下拉列表中，选择访问策略。

8. 单击**添加机密**。



#### 审查对组织级机密的访问

您可以检查将哪些访问策略应用于组织中的机密。

1. 在GitHub上，导航到组织的主页。

2. 在您的组织名称下，点击 **设置**。

   ![img](/organization-settings-tab.png) 

3. 在左侧边栏中，点击**机密**。

4. 机密列表包括所有已配置的权限和策略。例如：

   ![img](/actions-org-secrets-list.png) 

5. 有关为每个机密配置的权限的更多详细信息，请单击**更新**。



#### 在工作流程中使用加密的机密

除了 `GITHUB_TOKEN`，当工作流从Fork的存储库触发时，机密不会传递给运行者。

要将机密作为输入或环境变量提供给 动作，可以使用`secrets`上下文访问在存储库中创建的机密。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)”和“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions)”。

```yaml
steps:
  - name: Hello world action
    with: # 将机密设置为输入
      super_secret: ${{ secrets.SuperSecret }}
    env: # 或者作为一个环境变量
      super_secret: ${{ secrets.SuperSecret }}
```

尽可能避免从命令行在进程之间传递机密。命令行过程可能对其他用户可见（使用`ps`命令）或由[安全审核事件](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/manage/component-updates/command-line-process-auditing)捕获。为了帮助保护机密，请考虑使用环境变量、`STDIN`或目标进程支持的其他机制。

如果必须在命令行中传递机密，则将其包含在适当的引用规则中。机密通常包含特殊字符，它们可能会无意间影响您的shell。要转义这些特殊字符，请在环境变量中使用**引号**。例如：

**使用Bash的示例** 

```yaml
steps:
  - shell: bash
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "$SUPER_SECRET"
```

**使用PowerShell的示例**

```yaml
steps:
  - shell: pwsh
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "$env:SUPER_SECRET"
```

**使用Cmd.exe的示例**

```yaml
steps:
  - shell: cmd
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "%SUPER_SECRET%"
```

#### 机密限制

您的工作流程最多可以包含100个机密。机密环境变量的名称在存储库中必须唯一。

机密限制为64 KB。要使用大于64 KB的机密，可以将加密的机密存储在存储库中，并将解密密码短语另存为GitHub上的机密。例如，在将文件检入GitHub上的存储库之前，可以使用本地`gpg`对凭据进行加密。有关更多信息，请参见“ [gpg联机帮助页”](https://www.gnupg.org/gph/de/manual/r1023.html)。

> **警告**：请注意，在执行动作时不会泄露您的机密。使用此解决方法时，GitHub不会编辑日志中打印的机密。

1. 从终端运行以下命令，使用`gpg`和`AES256`密码算法加密 `my_secret.json`  文件。

   ```shell
   $ gpg --symmetric --cipher-algo AES256 my_secret.json
   ```

2. 系统将提示您输入密码。请记住密码，因为您需要在GitHub上创建一个新机密，将密码用作值。

3. 创建一个包含密码短语的新机密。例如，使用名称`LARGE_SECRET_PASSPHRASE`创建一个新机密并将机密的值设置为您在上述步骤中选择的密码短语。

4. 将加密的文件复制到存储库中并提交。在此示例中，加密文件为`my_secret.json.gpg`。

5. 创建一个shell脚本来解密密码。将此文件另存为`decrypt_secret.sh`。

   ```bash
   #!/bin/sh
   
   # 解密文件
   mkdir $HOME/secrets
   # --阻止交互命令的批处理
   # --回答问题时假设"yes"
   gpg --quiet --batch --yes --decrypt --passphrase="$LARGE_SECRET_PASSPHRASE" \
   --output $HOME/secrets/my_secret.json my_secret.json.gpg
   ```

6. 在将其检入存储库之前，请确保您的Shell脚本可执行。

   ```shell
   $ chmod +x decrypt_secret.sh
   $ git add decrypt_secret.sh
   $ git commit -m "Add new decryption script"
   $ git push
   ```

7. 在您的工作流程中，使用`step`来调用Shell脚本并解密密码。要在运行工作流的环境中拥有存储库的副本，则需要使用[`actions/checkout`](https://github.com/actions/checkout)动作。使用相对于存储库根目录的`run`命令来引用您的Shell脚本。

   ```yaml
   name: Workflows with large secrets
   
   on: push
   
   jobs:
     my-job:
       name: My Job
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         - name: Decrypt large secret
           run: ./.github/scripts/decrypt_secret.sh
           env:
             LARGE_SECRET_PASSPHRASE: ${{ secrets.LARGE_SECRET_PASSPHRASE }}
         # 这个命令只是一个显示你的秘密被打印的例子
         # 确保你删除了你的秘密的任何打印声明。GitHub不会隐藏使用此解决方案的秘密。
         - name: Test printing your secret (Remove this step in production)
           run: cat $HOME/secrets/my_secret.json
   ```



### 创建依赖作业

> 参阅：[管理复杂的工作流程](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/managing-complex-workflows)  [md](GitHubActions.Docs/learn-github-actions/managing-complex-workflows.md)

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

### 缓存依赖

> 参阅：[管理复杂的工作流程](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/managing-complex-workflows)  [md](GitHubActions.Docs/learn-github-actions/managing-complex-workflows.md)

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

### 使用数据库和服务容器

> 参阅：[管理复杂的工作流程](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/managing-complex-workflows)  [md](GitHubActions.Docs/learn-github-actions/managing-complex-workflows.md)

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

### 查找和自定义动作

> 参阅：[查找和自定义Actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions)   [md](GitHubActions.Docs/learn-github-actions/finding-and-customizing-actions.md)

GitHub Marketplace是您查找GitHub社区创建的动作的中心位置。[GitHub Marketplace页面](https://github.com/marketplace/actions/)使您可以按类别过滤动作。

#### 在工作流编辑器中浏览Marketplace动作

您可以直接在存储库的工作流编辑器中搜索和浏览动作。您可以从边栏中搜索特定动作，查看特色动作并浏览特色类别。您还可以查看从GitHub社区收到的动作数量。

1. 在存储库中，浏览到要编辑的工作流程文件。

2. 在文件视图的右上角，打开，点击![1604079392744](/1604079392744.png)。

   ![img](/actions-edit-workflow-file.png) 

3. 在编辑器的右侧，使用GitHub Marketplace侧栏浏览动作。带有![1604079247118](/1604079247118.png)徽章的动作表明GitHub已验证该动作的创建者为合作伙伴组织。 

   ![img](/actions-marketplace-sidebar.png) 



#### 向您的工作流程添加动作

动作的列表页面包括该动作的版本和使用该动作所需的工作流语法。为了即使在对动作进行更新时也能保持工作流程稳定，您可以通过在工作流程文件中指定Git或Docker标记号来引用要使用的动作的版本。

1. 导航到要在工作流中使用的动作。

2. 在“安装”下，单击![1604079448302](/1604079448302.png)以复制工作流程语法。

   ![img](/actions-sidebar-detailed-view.png) 

3. 将语法粘贴为工作流程中的新步骤。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idsteps)”。
4. 如果动作要求您提供输入，请在工作流程中进行设置。有关动作可能需要的输入的信息，请参阅“[将输入和输出与动作一起使用](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions#using-inputs-and-outputs-with-an-action)”。

您还可以为添加到工作流中的动作启用GitHub Dependabot版本更新。有关更多信息，请参阅“[使用GitHub Dependabot保持最新动作](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/keeping-your-actions-up-to-date-with-github-dependabot)”。 



#### 使用发布管理进行自定义动作

社区动作的创建者可以选择使用标签，分支或SHA值来管理动作的发布。与任何依赖项类似，您应该根据自己的舒适程度（自动接受动作更新）来指定要使用的动作版本。

您将在工作流文件中指定动作的版本。查看动作的文档以获取有关其发行版管理方法的信息，并查看要使用的标记，分支或SHA值。

##### 使用标签

标签对于让您决定何时在主要版本和次要版本之间进行切换很有用，但是这些标记只是暂时的，可以由维护者移动或删除。此示例演示了如何定位标记为`v1.0.1`的动作：

```yaml
steps:
    - uses: actions/javascript-action@v1.0.1
```

##### 使用SHA

如果需要更可靠的版本控制，则应使用与动作版本关联的SHA值。SHA是不可变的，因此比标记或分支更可靠。但是，这种方法意味着您将不会自动收到某项动作的更新，包括重要的错误修复和安全更新。本示例以动作的SHA为目标：

```yaml
steps:
    - uses: actions/javascript-action@172239021f7ba04fe7327647b213799853a9eb89
```

##### 使用分支

提及特定分支意味着该动作将始终使用目标分支上的最新更新，但是如果这些更新包含重大更改，则可能会产生问题。本示例针对一个名为`@main`的分支：

```yaml
steps:
    - uses: actions/javascript-action@main
```

有关更多信息，请参阅“[对动作使用版本管理](https://docs.github.com/en/free-pro-team@latest/actions/creating-actions/about-actions#using-release-management-for-actions)”。

#### 通过动作使用输入和输出

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

#### 在工作流文件使用动作的同一存储库中引用动作

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

#### 在Docker Hub上引用容器

如果在Docker Hub上的已发布Docker容器映像中定义了动作，则必须在工作流文件中使用`docker://{image}:{tag}`语法引用该动作。为了保护您的代码和数据，强烈建议您在工作流中使用Docker Hub之前，先验证Docker容器映像的完整性。

```yaml
jobs:
  my_first_job:
    steps:
      - name: My first step
        uses: docker://alpine:3.8
```

有关Docker动作的一些示例，请参阅[Docker-image.yml工作流程](https://github.com/actions/starter-workflows/blob/main/ci/docker-image.yml)和“[创建Docker容器动作”](https://docs.github.com/en/free-pro-team@latest/articles/creating-a-docker-container-action)。



### 管理工作流运行

>  参阅：[管理工作流运行](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs)  [md](GitHubActions.Docs/managing-workflow-runs/index.md)

#### 查看工作流程运行历史

执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**

   ![主存储库导航中的"动作"选项卡](/actions-tab.png)

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/workflow-sidebar.png)

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/run-name.png)





#### 使用工作流程运行日志

您可以从工作流运行页面查看工作流运行是正在进行还是已完成。您必须登录GitHub帐户才能查看工作流程运行信息，包括公共存储库。有关更多信息，请参阅“ [GitHub上的访问权限](https://docs.github.com/en/free-pro-team@latest/articles/access-permissions-on-github)”。

如果运行完成，则可以查看结果是成功，失败，已取消还是中立。如果运行失败，则可以查看和搜索构建日志以诊断失败并重新运行工作流。您还可以查看可计费的作业执行分钟，或下载日志并构建工件。

GitHub Actions使用Checks API输出工作流的状态，结果和日志。GitHub为每个工作流程运行创建一个新的检查套件。检查套件包含针对工作流程中每个作业的检查运行，并且每个作业都包含步骤。GitHub Actions作为工作流中的一个步骤运行。 有关Checks API的更多信息，请参见“ [Checks”](https://docs.github.com/en/free-pro-team@latest/v3/checks)。

> **注意：**确保仅将有效的工作流文件提交到存储库。如果`.github/workflows`包含无效的工作流文件，则GitHub Actions会为每个新提交生成失败的工作流运行。

##### 查看日志以诊断故障

如果您的工作流运行失败，则可以查看导致失败的步骤，并查看失败步骤的构建日志以进行故障排除。您可以看到每个步骤运行所花费的时间。您还可以复制日志文件中特定行的永久链接，以与团队共享。执行这些步骤需要对存储库具有读取权限。

除了在工作流文件中配置的步骤外，GitHub还向每个作业添加了两个附加步骤，以设置和完成作业的执行。这些步骤以名称“设置作业”和“完成作业”记录在工作流中。

对于在GitHub上托管的运行程序上运行的作业，“设置作业”记录了运行程序的虚拟环境的详细信息，并包括指向运行程序计算机上存在的预安装工具列表的链接。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/superlinter-workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/superlinter-run-name.png)

   

5. 在左侧边栏中，单击要查看的作业。

   ![Lint代码基础作业](/superlinter-lint-code-base-job.png)

   

6. 任何失败的步骤都会自动展开以显示结果。

   ![超级皮棉工作流程结果](/super-linter-workflow-results-updated.png)

   

7. （可选）要获得指向日志中特定行的链接，请单击步骤的行号。然后，您可以从Web浏览器的地址栏中复制链接。

   ![复制链接的按钮](/copy-link-button-updated.png)

   

##### 搜索日志

您可以在构建日志中搜索特定步骤。搜索日志时，结果中仅包含扩展步骤。执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/superlinter-workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/superlinter-run-name.png)

   

5. 在左侧边栏中，单击要查看的作业。

   ![Lint代码基础作业](/superlinter-lint-code-base-job.png)

   

6. 在日志输出的右上角的“**搜索日志”**搜索框中，键入搜索查询。

   ![搜索框搜索日志](/search-log-box-updated.png)

   

##### 下载日志

您可以从工作流程运行中下载日志文件。您还可以下载工作流程的工件。有关更多信息，请参见“[使用工件持久化工作流数据](https://docs.github.com/en/free-pro-team@latest/actions/automating-your-workflow-with-github-actions/persisting-workflow-data-using-artifacts)”。执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/superlinter-workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/superlinter-run-name.png)

   

5. 在左侧边栏中，单击要查看的作业。

   ![Lint代码基础作业](/superlinter-lint-code-base-job.png)

   

6. 点击右上角的，然后选择**下载日志存档**。

   ![下载日志下拉菜单](/download-logs-drop-down-updated.png)

   

##### 删除日志

您可以从工作流运行中删除日志文件。执行这些步骤需要对存储库进行写访问。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/superlinter-workflow-sidebar.png)

   

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/superlinter-run-name.png)

   

5. 点击右上角的。

   ![烤肉串水平图标](/workflow-run-kebab-horizontal-icon-updated.png)

   

6. 要删除日志文件，请单击“**删除所有日志”**按钮，然后查看确认提示。

   ![删除所有日志](/delete-all-logs-updated.png)

   删除日志后，将移除“**删除所有日志**”按钮，以指示工作流运行中没有剩余日志文件。



#### 手动运行工作流程

要手动运行工作流程，必须将工作流程配置为在`workflow_dispatch`事件上运行。有关更多信息，请参阅“[触发工作流的事件](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)”。

##### 在GitHub上运行工作流程

要在GitHub上触发`workflow_dispatch`事件，您的工作流程必须在默认分支中。请按照以下步骤手动触发工作流程运行。

执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 在左侧边栏中，单击要运行的工作流程。

   ![动作选择工作流程](/actions-select-workflow.png)

4. 在工作流程运行列表上方，选择**运行工作流程**。

   ![动作工作流调度](/actions-workflow-dispatch.png)

5. 选择工作流将在其中运行的分支，然后键入工作流使用的输入参数。点击**运行工作流程**。

   ![动作手动运行工作流程](/actions-manually-run-workflow.png)

##### 使用REST API运行工作流程

使用REST API时，您可以配置`inputs`和`ref`作为请求主体参数。如果省略输入，则使用工作流文件中定义的默认值。

有关使用REST API的更多信息，请参阅“[创建工作流调度事件”](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions/#create-a-workflow-dispatch-event)。



#### 重新运行工作流程

您可以重新运行工作流程的实例。重新运行工作流程使用与触发工作流程运行的原始事件相同的`GITHUB_SHA`（提交SHA）和`GITHUB_REF`（Git ref）。 

执行这些步骤需要对存储库具有读取权限。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/workflow-sidebar.png)

4. 从工作流运行的列表中，单击要查看的运行的名称。

   ![工作流程运行名称](/run-name.png)

5. 在工作流程的右上角，使用**重新运行作业**下拉菜单，然后选择**重新运行所有作业**。 

   ![重新运行检查下拉菜单](/rerun-checks-drop-down.png)



#### 禁用和启用工作流程

禁用工作流程可以使您停止触发工作流程，而不必从存储库中删除文件。您可以在GitHub上轻松轻松地重新启用工作流程。您还可以使用REST API禁用和启用工作流程。有关更多信息，请参阅“ [Actions REST API”](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#workflows)。

在许多情况下，临时禁用工作流可能很有用。这些是禁用工作流程可能会有所帮助的一些示例：

* 产生太多或错误请求的工作流程错误，会对外部服务产生负面影响。
* 工作流程不是很关键，占用您的帐户太多时间。
* 将请求发送到已关闭的服务的工作流。
* 分叉存储库上不需要的工作流（例如计划的工作流）。

> **警告：**为防止不必要的工作流程运行，计划的工作流程可能会自动禁用。派生公共存储库时，默认情况下禁用计划的工作流程。在公共存储库中，如果60天内未发生任何存储库活动，则计划的工作流将自动禁用。



##### 禁用工作流程

您可以手动禁用工作流程，以便它不会执行任何工作流程运行。禁用的工作流程不会被删除，可以重新启用。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![img](/actions-tab.png) 

3. 在左侧边栏中，单击要禁用的工作流程。 

   ![img](/actions-select-workflow.png) 

   4. 点击![1604249048477](/1604249048477.png)。 

      ![img](/actions-workflow-menu-kebab.png) 

   5. 点击**禁用工作流程**。 

      ![img](/actions-disable-workflow.png) 

已禁用的工作流程标记为![1604249110989](/1604249110989.png)以指示其状态。 

![img](/actions-find-disabled-workflow.png) 



##### 启用工作流程

您可以重新启用以前禁用的工作流程。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

3. 在左侧边栏中，单击要启用的工作流程。

   ![img](/actions-select-disabled-workflow.png)  

   

4. 点击**启用工作流程**。 

   ![img](/actions-enable-workflow.png) 





#### 删除工作流程运行

您可以删除已经完成或已超过两周的工作流程运行。 

执行这些步骤需要对存储库进行写访问。

1. 在GitHub上，导航到存储库的主页。

2. 在您的存储库名称下，点击**动作(Actions)**。

   ![主存储库导航中的"操作"选项卡](/actions-tab.png)

   

3. 在左侧边栏中，单击要查看的工作流程。

   ![左侧栏中的工作流程列表](/workflow-sidebar.png)

   

4. 要删除工作流程运行，请使用下拉菜单，然后选择**删除工作流程运行**。

    

   ![删除工作流程运行](/workflow-delete-run.png)

   

5. 查看确认提示，然后单击“**是，永久删除此工作流程运行”**。

    

   ![删除工作流程运行确认](/workflow-delete-run-confirmation.png)



#### 添加工作流状态标志

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

##### 使用工作流程名称

这个Markdown示例为名称为“ Greet Everyone”的工作流添加了状态徽章。该仓库的`OWNER`是`actions`组织和`REPOSITORY`名字是`hello-world`。

```
![example workflow name](https://github.com/actions/hello-world/workflows/Greet%20Everyone/badge.svg)
```

##### 使用工作流程文件路径

这个Markdown示例为带有文件路径`.github/workflows/main.yml`的工作流添加了状态标记。该仓库的`OWNER`是`actions`组织和`REPOSITORY`名字是`hello-world`。

```
![example workflow file path](https://github.com/actions/hello-world/workflows/.github/workflows/main.yml/badge.svg)
```

##### 使用`branch`参数

这个Markdown示例为名称为`feature-1`的分支添加了状态徽章。

```
![example branch parameter](https://github.com/actions/hello-world/workflows/Greet%20Everyone/badge.svg?branch=feature-1)
```

##### 使用`event`参数

这个Markdown示例添加了一个徽章，用于显示由`pull_request`事件触发的工作流运行状态。

```
![example event parameter](https://github.com/actions/hello-world/workflows/Greet%20Everyone/badge.svg?event=pull_request)
```





### 工作流程中的身份验证

> 参阅：[工作流程中的身份验证](https://docs.github.com/en/free-pro-team@latest/actions/reference/authentication-in-a-workflow)  [md](GitHubActions.Docs/reference/authentication-in-a-workflow.md)

> GitHub提供了一个令牌，您可以使用该令牌代表GitHub Actions进行身份验证。 

有`write`权访问存储库的任何人都可以创建，读取和使用机密。

#### 关于`GITHUB_TOKEN`机密

GitHub自动创建一个可在您的工作流程中使用的`GITHUB_TOKEN`机密。您可以在工作流运行中使用`GITHUB_TOKEN`进行身份验证。

启用GitHub Actions后，GitHub会在您的存储库中安装GitHub App。该`GITHUB_TOKEN`机密是GitHub的应用程序安装的访问令牌。您可以使用安装访问令牌代表存储库中安装的GitHub App进行身份验证。令牌的权限仅限于包含您的工作流程的存储库。有关更多信息，请参见[`GITHUB_TOKEN`权限](https://docs.github.com/en/free-pro-team@latest/actions/reference/authentication-in-a-workflow#permissions-for-the-github_token)。

在每个作业开始之前，GitHub会获取该作业的安装访问令牌。作业完成后，令牌将过期。

该令牌在`github.token`上下文中也可用。有关更多信息，请参阅“ [GitHub Actions的上下文和表达式语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#github-context)”。

#### 使用`GITHUB_TOKEN`工作流

要使用`GITHUB_TOKEN`机密，必须在工作流程文件中引用它。使用令牌可能包括将令牌作为输入传递给需要它的动作，或者进行经过身份验证的GitHub API调用。

当您使用存储库的`GITHUB_TOKEN`代表GitHub Actions应用程序执行任务时，由`GITHUB_TOKEN`触发的事件将不会创建新的工作流程运行。这样可以防止您意外创建递归工作流运行。例如，如果工作流运行使用存储库的`GITHUB_TOKEN`推送代码，即使存储库包含配置为在`push`事件发生时运行的工作流，新的工作流也不会运行。

##### `GITHUB_TOKEN`作为输入传递的示例

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

##### 调用REST API的示例

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

####  `GITHUB_TOKEN`的权限

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



### GitHub托管的运行者规格

> 参阅：[GitHub托管的运行者规格](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners)  [md](GitHubActions.Docs/reference/specifications-for-github-hosted-runners.md)

> GitHub提供了托管虚拟机来运行工作流。虚拟机包含可供GitHub Actions使用的工具，软件包和设置的环境。 

#### 关于GitHub托管的运行者

GitHub托管的运行程序是由GitHub托管并安装了GitHub Actions运行程序应用程序的虚拟机。GitHub为运行Linux，Windows和macOS操作系统的用户提供帮助。

当您使用GitHub托管的运行程序时，机器维护和升级将由您来完成。您可以直接在虚拟机或Docker容器中运行工作流程。

您可以为工作流程中的每个作业指定运行者类型。工作流中的**每个作业都在虚拟机的新实例中执行**。作业中的所有步骤都在虚拟机的同一实例中执行，从而允许该作业中的动作使用文件系统共享信息。

GitHub Actions运行器应用程序是开源的。你可以作出贡献，并在文件问题在 [runner](https://github.com/actions/runner)  库。

##### 面向GitHub托管的运行者的云主机

GitHub在Microsoft Azure的Standard_DS2_v2虚拟机上托管Linux和Windows运行程序，并安装了GitHub Actions运行程序应用程序。GitHub上托管的运行器应用程序是Azure Pipelines代理的分支。所有Azure虚拟机的入站ICMP数据包均被阻止，因此ping或traceroute命令可能不起作用。有关Standard_DS2_v2计算机资源的更多信息，请参见Microsoft Azure文档中的“ [Dv2和DSv2系列](https://docs.microsoft.com/azure/virtual-machines/dv2-dsv2-series#dsv2-series)”。

GitHub使用[MacStadium](https://www.macstadium.com/)托管macOS运行程序。

##### GitHub托管的运行者的管理特权

Linux和macOS虚拟机均使用无密码运行`sudo`。当您需要执行命令或安装需要比当前用户更多特权的工具时，可以使用`sudo`而无需提供密码。有关更多信息，请参见“ [Sudo手册”](https://www.sudo.ws/man/1.8.27/sudo.man.html)。

Windows虚拟机被配置为以禁用了用户帐户控制（UAC）的管理员身份运行。有关更多信息，请参见Windows文档中的“[用户帐户控制的工作方式](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/how-user-account-control-works)”。

#### 受支持的运行者和硬件资源

每个虚拟机具有相同的可用硬件资源。

* 2核CPU
* 7 GB的RAM内存
* 14 GB的SSD磁盘空间

| 虚拟环境              | YAML工作流程标签                   |
| --------------------- | ---------------------------------- |
| Windows Server 2019   | `windows-latest` 或 `windows-2019` |
| Ubuntu 20.04          | `ubuntu-20.04`                     |
| Ubuntu 18.04          | `ubuntu-latest` 或 `ubuntu-18.04`  |
| Ubuntu 16.04          | `ubuntu-16.04`                     |
| macOS Big Sur 11.0    | `macos-11.0`                       |
| macOS  Catalina 10.15 | `macos-latest` 或 `macos-10.15`    |

> **注意：**当前仅提供Ubuntu 20.04虚拟环境的预览。该`ubuntu-latest`YAML工作流标签仍然采用了Ubuntu 18.04虚拟环境。
>
> **注意：** MacOS 11.0虚拟环境当前仅作为预览提供。该`macos-latest`YAML工作流标签仍然采用MacOS的10.15虚拟环境。

工作流日志列出了用于运行作业的运行者。有关更多信息，请参阅“[查看工作流运行历史记录”](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/viewing-workflow-run-history)。

#### 支持软件

GitHub托管的运行程序中包含的软件工具每周更新一次。有关每个运行程序操作系统随附的工具的最新列表，请参见以下链接：

* [Ubuntu 20.04 LTS](https://github.com/actions/virtual-environments/blob/main/images/linux/Ubuntu2004-README.md)
* [Ubuntu 18.04 LTS](https://github.com/actions/virtual-environments/blob/main/images/linux/Ubuntu1804-README.md)
* [Ubuntu 16.04 LTS](https://github.com/actions/virtual-environments/blob/main/images/linux/Ubuntu1604-README.md)
* [Windows Server 2019](https://github.com/actions/virtual-environments/blob/main/images/win/Windows2019-Readme.md)
* [Windows Server 2016](https://github.com/actions/virtual-environments/blob/main/images/win/Windows2016-Readme.md)
* [MacOS 10.15](https://github.com/actions/virtual-environments/blob/main/images/macos/macos-10.15-Readme.md)
* [MacOS 11.0](https://github.com/actions/virtual-environments/blob/main/images/macos/macos-11.0-Readme.md)

> **注意：**当前仅提供Ubuntu 20.04虚拟环境的预览。该`ubuntu-latest`YAML工作流标签仍然采用了Ubuntu 18.04虚拟环境。
>
> **注意：** MacOS 11.0虚拟环境当前仅作为预览提供。该`macos-latest`YAML工作流标签仍然采用MacOS的10.15虚拟环境。

GitHub托管的运行程序，除了上述参考文献中列出的软件包之外，还包括操作系统的默认内置工具。例如，Ubuntu的和MacOS包括`grep`，`find`，和`which`，在其他默认工具中。 

工作流日志包括指向运行器上预装工具的链接。有关更多信息，请参阅“[查看工作流运行历史记录”](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/viewing-workflow-run-history)。

如果有您要使用的工具，请在[action / virtual-environments中](https://github.com/actions/virtual-environments)打开一个问题。

#### IP地址

> **注意：**如果您为GitHub组织或企业帐户使用IP地址允许列表，则不能使用GitHub托管的运行程序，而必须使用自托管的运行程序。有关更多信息，请参见“[关于自托管赛跑者”](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners)。

Windows和Ubuntu运行程序托管在Azure中，并且具有与Azure数据中心相同的IP地址范围。当前，所有Windows和Ubuntu GitHub托管的运行程序都位于以下Azure区域：

* 美国东部（`eastus`）
* 美国东部2（`eastus2`）
* 美国西部2（`westus2`）
* 美国中部（`centralus`）
* 美国中南部（`southcentralus`）

Microsoft每周更新一次JSON文件中的Azure IP地址范围，您可以从[Azure IP范围和服务标签-公共云](https://www.microsoft.com/en-us/download/details.aspx?id=56519)网站下载该文件。如果需要允许列表以防止未经授权访问内部资源，则可以使用此IP地址范围。

JSON文件包含一个名为`values`的数组。在该阵列内，您可以在带有`name`和`id`的Azure区域的对象中找到支持的IP地址，例如`"AzureCloud.eastus2"`。

您可以在`"addressPrefixes"`对象中找到受支持的IP地址范围。这是JSON文件的精简示例。

```json
{
  "changeNumber": 84,
  "cloud": "Public",
  "values": [
    {
      "name": "AzureCloud.eastus2",
      "id": "AzureCloud.eastus2",
      "properties": {
        "changeNumber": 33,
        "region": "eastus2",
        "platform": "Azure",
        "systemService": "",
        "addressPrefixes": [
          "13.68.0.0/17",
          "13.77.64.0/18",
          "13.104.147.0/25",
          ...
        ]
      }
    }
  ]
}
```

#### 文件系统

GitHub在虚拟机上的特定目录中执行动作和Shell命令。虚拟机上的文件路径不是静态的。使用环境变量的GitHub提供了构建文件路径为`home`，`workspace`和`workflow`目录。

| 目录                  | 环境变量            | 描述                                                         |
| --------------------- | ------------------- | ------------------------------------------------------------ |
| `home`                | `HOME`              | 包含与用户相关的数据。例如，此目录可能包含来自登录尝试的凭据。 |
| `workspace`           | `GITHUB_WORKSPACE`  | 动作和外壳命令在此目录中执行。一个动作可以修改此目录的内容，以后的动作可以访问该目录。 |
| `workflow/event.json` | `GITHUB_EVENT_PATH` | `POST`触发工作流的webhook事件的有效负载。每次执行动作以隔离动作之间的文件内容时，GitHub都会对此进行重写。 |

有关GitHub为每个工作流创建的环境变量的列表，请参阅“[使用环境变量”](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/using-environment-variables)。

##### Docker容器文件系统

在Docker容器中运行的动作在`/github`路径下具有静态目录。但是，我们强烈建议使用默认环境变量在Docker容器中构造文件路径。

GitHub保留`/github`路径前缀，并为动作创建三个目录。

* `/github/home`
* `/github/workspace`-**注意：** GitHub Actions必须由默认Docker用户（root）运行。确保您的Dockerfile没有设置`USER`指令，否则您将无法访问`GITHUB_WORKSPACE`。
* `/github/workflow`



## 更多

更多内容，参阅

[学习GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions)   [md](GitHubActions.Docs/learn-github-actions/index.md)

* [GitHub Actions的安全性强化](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/security-hardening-for-github-actions)  [md](GitHubActions.Docs/learn-github-actions/security-hardening-for-github-actions.md)

  使用GitHub Actions功能的良好安全做法。



[管理工作流运行](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs)  [md](GitHubActions.Docs/managing-workflow-runs/index.md)

* [取消工作流程](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/canceling-a-workflow)   [md](GitHubActions.Docs/managing-workflow-runs/canceling-a-workflow.md)

* [查看作业执行时间](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/viewing-job-execution-time)   [md](GitHubActions.Docs/managing-workflow-runs/viewing-job-execution-time.md)

  

[参考](https://docs.github.com/en/free-pro-team@latest/actions/reference)   [md](GitHubActions.Docs/reference/index.md)

* [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions)   [md](GitHubActions.Docs/reference/workflow-syntax-for-github-actions.md)

* [GitHub Actions的上下文和表达语法](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)    [md](GitHubActions.Docs/reference/context-and-expression-syntax-for-github-actions.md)

* [GitHub Actions的工作流程命令](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-commands-for-github-actions)    [md](GitHubActions.Docs/reference/workflow-commands-for-github-actions.md)

* [触发工作流程的事件](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)   [md](GitHubActions.Docs/reference/events-that-trigger-workflows.md)

  



## 参考阅读

[^1]: [GitHub Actions 入门教程 - 阮一峰](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html) 

[关于GitHub动作的计费](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions)      [md](GitHubActions.Docs/others/about-billing-for-github-actions.md)



