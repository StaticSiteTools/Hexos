---
title: TravisCI
date: 2020-09-27  9:00:23
updated: 2020-09-27  9:00:23
mathjax: false
categories: 
tags:
typora-root-url: TravisCI
typora-copy-images-to: TravisCI
top: 1
comments: false
---


# TravisCI

[原文](https://docs.travis-ci.com/)



## 序言

**Travis CI** ： Travis  持续集成平台 

作为一个持续集成平台，Travis CI通过自动构建和测试代码更改来支持您的开发过程，并提供有关更改成功的即时反馈。Travis CI还可以通过管理部署和通知来自动化开发过程的其他部分。 



本文为摘录官方文档的部分内容，详细的请参阅  [官方文档](https://docs.travis-ci.com/)



## 使用

> 参阅： [Travis CI教程](https://docs.travis-ci.com/user/tutorial)

### 先决条件

要开始使用Travis CI，请确保您具有：

* 一个[GitHub](https://github.com/)或[Bitbucket](https://bitbucket.org/) 或[GitLab](https://about.gitlab.com/)或[Assembla](https://www.assembla.com/)帐户。
* 托管在[GitHub](https://help.github.com/categories/importing-your-projects-to-github/)或[Bitbucket](https://confluence.atlassian.com/bitbucket/transfer-repository-ownership-289964397.html)或[GitLab](https://www.tutorialspoint.com/gitlab/gitlab_user_permissions.htm)或[Assembla](https://articles.assembla.com/en/articles/1665737-advanced-user-permissions-controls)上的项目的所有者权限。



### 使用GitHub开始使用Travis CI 

1. 转到[Travis-ci.com](https://travis-ci.com/)并[使用GitHub注册](https://travis-ci.com/signin)。

2. 接受Travis CI的授权。您将被重定向到GitHub。

3. 单击Travis仪表板右上方的个人资料图片，单击“设置”，然后单击绿色的“*激活”*按钮，然后选择要用于Travis CI的存储库。

   > 或者您单击入门页面上的*使用GitHub Apps激活所有存储库*按钮以仅激活您的*所有存储库* 

4. 将`.travis.yml`文件添加到存储库中以告诉Travis CI怎么做。 

   以下示例指定了应使用Ruby 2.2和最新版本的JRuby构建的Ruby项目。

   ```YAML
   language: ruby
   rvm:
    - 2.2
    - jruby
   ```

   对于Ruby项目的默认值是`bundle install`用于[安装的依赖关系](https://docs.travis-ci.com/user/job-lifecycle/#customizing-the-installation-phase)，并`rake`构建项目。

5. 将`.travis.yml`文件添加到git中，提交并推送以触发Travis CI构建：

   > Travis仅在添加`.travis.yml`文件后推送的提交基础上运行。

6. 通过访问[Travis CI](https://travis-ci.com/auth)并选择您的存储库，检查构建状态页面，以根据构建命令的返回状态查看您的构建是否[通过或失败](https://docs.travis-ci.com/user/job-lifecycle/#breaking-the-build)。



### 使用GitLab开始使用Travis CI 

> 本节介绍了当前处于测试版中的新GitLab选项。 

1. 转到[Travis-ci.com](https://travis-ci.com/)并[使用GitLab进行注册](https://travis-ci.com/signin)。

2. 接受Travis CI的授权。您将被重定向到GitLab。

3. 单击Travis仪表板右上角的个人资料图片，单击*“设置”*，然后切换要与Travis CI一起使用的存储库。

4. 将`.travis.yml`文件添加到存储库中以告诉Travis CI怎么做。 

   以下示例指定了应使用Ruby 2.2和最新版本的JRuby构建的Ruby项目。

   ```yaml
   language: ruby
   rvm:
    - 2.2
    - jruby
   ```

   对于Ruby项目的默认值是`bundle install`用于[安装的依赖关系](https://docs.travis-ci.com/user/job-lifecycle/#customizing-the-installation-phase)，并`rake`构建项目。

5. 将`.travis.yml`文件添加到git中，提交并推送以触发Travis CI构建：

   > Travis仅在添加`.travis.yml`文件后推送的提交基础上运行。

6. 通过访问[Travis CI](https://travis-ci.com/auth)并选择您的存储库，检查构建状态页面，以根据构建命令的返回状态查看您的构建是否[通过或失败](https://docs.travis-ci.com/user/job-lifecycle/#breaking-the-build)。



### 选择不同的编程语言

使用以下常见语言之一：

`.travis.yml`

```yaml
language: ruby
```

```yaml
language: java
```

```yaml
language: node_js
```

```yaml
language: python
```

```yaml
language: php
```

```yaml
language: go
```

如果您有需要在macOS上运行的测试，或者您的项目使用Swift或Objective-C，请使用我们的macOS环境：

```yaml
os: osx
```

Travis CI支持多种[编程语言](https://docs.travis-ci.com/user/languages/)。 



## 生命周期

> 参阅： [工作生命周期](https://docs.travis-ci.com/user/job-lifecycle/)

Travis CI为每种编程语言提供了默认的构建环境和默认的阶段集。将使用您的工作的构建环境创建一个虚拟机，将您的存储库克隆到其中，安装可选的附加组件，然后运行构建阶段。 

### 构建

该`.travis.yml`文件描述了构建过程。 Travis CI中的**构建**是一系列[阶段](https://docs.travis-ci.com/user/build-stages)。每个**阶段** 都由并行运行的**作业**组成。 

### 工作生命周期 

每个工作都是一系列的**[阶段](https://docs.travis-ci.com/user/for-beginners/#builds-jobs-stages-and-phases)**。主要阶段包括： 

1. `install` -安装所需的任何依赖项
2. `script` - 运行构建脚本

Travis CI可以在以下阶段中运行自定义命令：

1. `before_install` -在安装阶段之前
2. `before_script` -在脚本阶段之前
3. `after_script` - 在脚本阶段之后。
4. `after_success`-构建*成功时*（例如，构建文档），结果在`TRAVIS_TEST_RESULT`环境变量中
5. `after_failure`-当构建*失败*（例如，上传日志文件）时，结果在`TRAVIS_TEST_RESULT`环境变量中



工作阶段的完整序列就是生命周期。步骤如下： 

1. 可选安装 [`apt addons`](https://docs.travis-ci.com/user/installing-dependencies/#installing-packages-with-the-apt-addon)
2. 可选安装 [`cache components`](https://docs.travis-ci.com/user/caching)
3. `before_install`
4. `install`
5. `before_script`
6. `script`
7. 可选`before_cache`（当且仅当缓存有效时）
8. `after_success` 要么 `after_failure`
9. 可选的`before_deploy`（当且仅当部署处于活动状态时）
10. 可选的 `deploy`
11. 可选的`after_deploy`（当且仅当部署处于活动状态时）
12. `after_script`

> 一个**构建**可以包含许多工作。 



### 自定义install阶段

默认的依赖项安装命令取决于项目语言。 您可以指定自己的脚本来安装项目依赖项在`.travis.yml`文件：  

```yaml
install: ./install-dependencies.sh
```

> 当使用自定义脚本，则它们应该是可执行文件（例如，使用`chmod +x`）和包含有效shebang行如`/usr/bin/env sh`，`/usr/bin/env ruby`，或`/usr/bin/env python`。 

您还可以提供多个步骤，例如安装ruby和node依赖项：

```yaml
install:
  - bundle install --path vendor/bundle
  - npm install
```

当安装步骤之一失败时，构建将立即停止并标记为[error](https://docs.travis-ci.com/user/job-lifecycle/#breaking-the-build)。

您还可以使用`apt-get`或`snap`来[安装依赖](https://docs.travis-ci.com/user/installing-dependencies/)

**跳过安装阶段**

通过将以下内容添加到您的`.travis.yml`内容中，完全跳过安装步骤：

```yaml
install: skip
```



### 自定义script阶段

默认的build命令取决于项目语言。Ruby项目使用`rake`，这是大多数Ruby项目的共同点。

您可以在`.travis.yml`中覆盖默认的构建步骤：

```yaml
script: bundle exec thor build
```

您还可以指定多个脚本命令：

```yaml
script:
- bundle exec rake build
- bundle exec rake builddoc
```

当其中一个构建命令返回非零退出代码时，Travis CI构建也会运行后续命令并累积构建结果。

在上面的示例中，如果`bundle exec rake build`返回退出代码1，`bundle exec rake builddoc`则仍在运行以下命令，但是该构建将导致失败。

如果第一步是运行单元测试，然后是集成测试，则您可能仍想查看单元测试失败时集成测试是否成功。

您可以通过使用一些Shell Magic来随后运行所有命令来更改此行为，但是当第一个命令返回非零退出代码时，构建仍然失败。这是您的`.travis.yml`摘录

```yaml
script: bundle exec rake build && bundle exec rake builddoc
```

此示例（请注意`&&`）`bundle exec rake build`失败时会立即失败。



#### 注意`$?`

`script`  中的每个命令都由一个特殊的bash函数处理。 这个函数操纵 `$?`  产生适合于显示的日志。

因此，您不应该依赖`script` 部分中 `$?`  的值来改变构建行为。

有关 更多技术讨论，请参[见此GitHub问题](https://github.com/travis-ci/travis-ci/issues/3771)。 



#### 复杂的编译命令

如果您有一个很难在`.travis.yml`中配置的复杂构建环境，请考虑将这些步骤移至单独的shell脚本中。该脚本可以作为存储库的一部分，并且可以从`.travis.yml`中轻松调用。 

考虑一个您想运行更复杂的测试方案的方案，但只针对不是来自请求请求的构建。Shell脚本可能是：

```bash
#!/bin/bash
set -ev
bundle exec rake:units
if [ "${TRAVIS_PULL_REQUEST}" = "false" ]; then
  bundle exec rake test:integration
fi
```

> 请注意顶部的`set -ev`。`-e`  一旦一个命令返回非零退出代码，该标志将导致脚本退出。如果您想要尽早退出任何脚本，这可能会很方便。它还有助于复杂的安装脚本，在该脚本中，一个失败的命令不会导致安装失败。`-v`标志使shell程序在执行之前打印脚本中的所有行，这有助于确定哪些步骤失败了。 

要从您的`.travis.yml`中运行该脚本：

1. 将其另存为您的存储库中的`scripts/run-tests.sh`。

2. 通过运行`chmod ugo+x scripts/run-tests.sh`使其可执行。

3. 提交到您的存储库。

4. 将其添加到您的`.travis.yml`

   ```yaml
    script: ./scripts/run-tests.sh
   ```



#### 这是如何运作的？

在工作生命周期中指定的步骤被编译成单个bash脚本并在工作程序上执行。

覆盖这些步骤时，请勿使用shell内置命令`exit`。这样做会冒着终止构建过程的风险，而不会给Travis提供执行后续任务的机会。

> 使用`exit`在自定义脚本内是安全的。如果指示错误，则任务将被标记为失败。



## 构建破坏

> 参阅： [工作生命周期](https://docs.travis-ci.com/user/job-lifecycle/)

如果工作生命周期的前四个阶段中的任何命令返回非零退出代码，则构建将被破坏：

* 如果`before_install`，`install`或`before_script`返回一个非零退出代码，编译时**出错**并立即停止。
* 如果`script`返回非零退出代码，则构建会**失败**，但是会继续运行，然后标记为**failed**。

`after_success`，`after_failure`，`after_script`，`after_deploy`的退出代码和后续阶段不会影响构建结果。但是，如果这些阶段之一超时，则将构建标记为**failed**。



## 自定义构建

> 参阅：[自定义构建](https://docs.travis-ci.com/user/customizing-the-build/)

### 仅构建最新的提交

如果仅对每个分支上的最新提交感兴趣，则可以使用此新功能自动取消队列中*尚未运行的*较早版本。现有版本将被允许完成。 

该**自动取消(Auto Cancellation)**设定是在每个存储库的设定选项卡，您可以单独启用它： 

* **自动取消分支构建(Auto cancel branch builds)** - 取消分支中排队的构建，并显示在存储库的“构建历史”选项卡中。
* **自动取消拉取请求构建(Auto cancel pull request builds)** - 取消拉取请求的排队构建（针对其目标的更改/功能分支的未来合并结果），并显示在存储库的“拉取请求”选项卡中。



### Git克隆深度

Travis CI可以将存储库克隆到50次提交的最大深度，这仅在执行git操作时才真正有用。 

> 请注意，如果使用深度为1并且有一个作业队列，则在推送新提交时，Travis CI不会构建队列中的提交。

您可以设置克隆深度在`.travis.yml`：

```yaml
git:
  depth: 3
```

您还可以使用以下方法完全删除`--depth`标志在`.travis.yml`中：

```yaml
git:
  depth: false
```

> 由于有限的git clone深度，无法访问存储库中的所有对象，因此存储库上的某些操作（例如常见的自动代码查看脚本（例如Ruby的Pronto））可能会失败。删除深度标志或运行`git fetch --unshallow`可能会解决此问题。 



### Git静默克隆

默认情况下，Travis CI克隆不带静默标志（`-q`）的存储库。如果您试图避免日志文件的大小限制，或者即使您不需要包含它，启用静默标志也将很有用。

您可以启用安静的标志在`.travis.yml`：

```yaml
git:
  quiet: true
```



### Git子模块

Travis CI默认情况下会克隆Git子模块。为了避免这种情况： 

```yaml
git:
  submodules: false
```



### Git LFS

#### 与GitHub认证

我们建议在使用[Git LFS](https://git-lfs.github.com/)时使用只读的GitHub OAuth令牌进行身份验证：

```yaml
before_install:
- echo -e "machine github.com\n  login $GITHUB_TOKEN" > ~/.netrc
- git lfs pull
```

连接到私有存储库时需要此身份验证，并且在连接到开源存储库时可以防止速率限制。

LFS当前不支持部署密钥，因此，如上例所示，您应使用GitHub OAuth令牌进行身份验证。

#### 与GitLab认证

我们建议在使用[Git LFS](https://git-lfs.github.com/)时使用只读的GitLab OAuth令牌进行身份验证：

```yaml
before_install:
- echo -e "machine gitlab.com\n  login $GITLAB_TOKEN" > ~/.netrc
- git lfs pull
```

连接到私有存储库时需要此身份验证，并且在连接到开源存储库时可以防止速率限制。

LFS当前不支持部署密钥，因此，如上例所示，您应该使用GitLab OAuth令牌进行身份验证。



### Git稀疏检出

Travis CI支持`git`的[稀疏检出](https://git-scm.com/docs/git-read-tree#_sparse_checkout) 功能。要稀疏克隆存储库，请添加：

```yaml
git:
  sparse_checkout: skip-worktree-map-file
```


这里`skip-worktree-map-file`是与数据当前库中的现有文件的路径，你想投入`$GIT_DIR/info/sparse-checkout`的文件[Git的文档中描述的格式](https://git-scm.com/docs/git-read-tree#_sparse_checkout)。

其中`skip-worktree-map-file`是指向当前存储库中现有文件的路径，其中包含要放入Git文档中所述[格式](https://git-scm.com/docs/git-read-tree#_sparse_checkout)的`$GIT_DIR/info/sparse-checkout`文件中的数据。



### Git行尾转换

Travis CI克隆具有依赖于平台的 [core.autocrlf](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coreautocrlf)行为的存储库。可以通过 `.travis.yml`中的autocrlf属性修改此行为。有效值为`true`，`false`和`input`。 

要克隆存储库而不进行行尾转换，请添加： 

```yaml
git:
  autocrlf: input
```

这相当于在克隆存储库之前的 [`git config --global core.autocrlf input`](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coreautocrlf)  。



### 禁用git克隆

在某些工作流程中，例如构建阶段，跳过自动`git clone`步骤可能会有所帮助。

您可以通过 `.travis.yml`中添加以下内容来做到这一点：

```yaml
git:
  clone: false
```

> 请注意，如果使用此选项，则不会定义环境变量`TRAVIS_COMMIT_MESSAGE`。



### 设置符号链接选项

在某些情况下，如果将存储库同时用于Linux和Windows，则可能需要设置 [core.symlinks](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)选项。 

```yaml
git:
  symlinks: true
```



### 构建指定分支

Travis CI 使用来自包含Git提交的分支的`.travis.yml`文件来触发构建。使用安全列表包括分支，或使用阻止列表排除分支。 

> 请注意，在决定对某些分支进行安全列表或阻止列表时，还需要考虑自动[拉取请求构建](https://docs.travis-ci.com/user/pull-requests#double-builds-on-pull-requests)。

#### 安全列表或阻止列表分支

指定要使用安全列表构建的分支或不想构建的阻止列表分支在`.travis.yml`文件： 

```yaml
# 阻止列表
branches:
  except:
  - legacy
  - experimental

# 安全列表
branches:
  only:
  - master
  - stable
```

如果同时使用安全列表和阻止列表，则安全列表优先。默认情况下，`gh-pages`除非将其添加到安全列表中，否则不会构建该分支。

要建立*所有*分支：

```yaml
branches:
  only:
  - gh-pages
  - /.*/
```

#### 使用正则表达式

您可以使用正则表达式来安全列出或阻止列表分支：

```yaml
branches:
  only:
  - master
  - /^deploy-.*$/
```

分支列表中用`/`包围的任何名称都被视为正则表达式，并且可以包含[Ruby正则表达式](http://www.ruby-doc.org/core/Regexp.html)支持的任何量词，锚点或字符类。

不支持在最后一个`/`之后指定的选项（例如，`i`用于不区分大小写的匹配），但可以内联给定。例如， `/^(?i:deploy)-.*$/`  匹配`Deploy-2014-06-01`和`deploy-`以任何情况组合开头的其他分支和标记。 



### 跳过构建

如果出于某种原因不想为特定的提交运行构建，则可以指示Travis CI通过提交消息中的命令跳过构建此提交。

该命令应为以下格式之一：

```
[<KEYWORD> skip]
或
[skip <KEYWORD>]
```

其中`<KEYWORD>`是`ci`，`travis`，`travis ci`，`travis-ci`，或`travisci`。例如，

```
[skip travis] Update README
```

请注意，如果将多个提交一起推送，则仅当HEAD提交的提交消息中存在skip命令时，该命令才有效。 



### 实现复杂的构建步骤

如果您有一个很难在`.travis.yml`中配置的复杂构建环境，请考虑将这些步骤移至单独的shell脚本中。该脚本可以作为存储库的一部分，并且可以从`.travis.yml`中轻松调用。 



### 自定义主机名

如果构建需要设置自定义主机名，则可以在`.travis.yml`中指定单个主机或它们的列表。Travis CI将自动为IPv4和IPv6 设置主机名在`/etc/hosts`。 

```yaml
addons:
  hosts:
  - travis.test
  - joshkalderimis.com
```




## 环境变量

> 参阅： [环境变量](https://docs.travis-ci.com/user/environment-variables/)

定制构建过程的一种常见方法是定义环境变量，可以从构建过程的任何阶段访问这些变量。

定义环境变量的最佳方法取决于它将包含的信息类型以及何时需要更改它：

* 如果它*不*包含敏感信息，并且应该对forks可用 - [将它添加到您的.travis.yml](https://docs.travis-ci.com/user/environment-variables/#defining-public-variables-in-travisyml)
* 如果它*确实*包含敏感信息，并且对于所有分支都相同，[请对其进行加密，然后将其添加到您的.travis.yml中](https://docs.travis-ci.com/user/environment-variables/#defining-encrypted-variables-in-travisyml)
* 如果它*确实*包含敏感信息，并且对于不同的分支可能有所不同，[请将其添加到“存储库设置”中](https://docs.travis-ci.com/user/environment-variables/#defining-variables-in-repository-settings)

### 在`.travis.yml`定义公共变量

`.travis.yml`中定义的公共变量与某个提交相关联。更改它们需要重新提交，重新启动旧版本将使用旧值。它们也可以在存储库的分支上自动使用。

在`.travis.yml`中定义变量：

* 运行构建所需的文件，并且其中不包含敏感数据。例如，可能需要`$RACK_ENV`将Ruby应用程序的测试套件设置为`test`。
* 每个分支不同。
* 因工作而异。

在您`.travis.yml`的`env`键中定义环境变量，并引用特殊字符，例如星号（`*`）。`env`阵列中的每一行都会触发一次构建。

```yaml
env:
  - DB=postgres
  - SH=bash
  - PACKAGE_VERSION="1.0.*"
```

> 如果在`.travis.yml`和存储库设置中定义了同名的变量，则`.travis.yml`中的变量优先。如果在`.travis.yml`中将变量定义为加密和未加密，则文件中稍后定义的变量优先。

> 变量的值逐字传递到生成的生成脚本。因此，请确保相应地转义任何Bash特殊字符。特别是，如果值包含空格，则需要在该值两边加上引号。例如a long phrase应该写成"a long phrase"。

#### 全局变量

您可以使用`global`定义全局变量，使用`jobs`定义作业变量。

```yaml
env:
  global:
    - CAMPFIRE_TOKEN=abc123
    - TIMEOUT=1000
  jobs:
    - USE_NETWORK=true
    - USE_NETWORK=false
```

触发器具有以下`env`行：

```
USE_NETWORK=true CAMPFIRE_TOKEN=abc123 TIMEOUT=1000
USE_NETWORK=false CAMPFIRE_TOKEN=abc123 TIMEOUT=1000
```



### 在`.travis.yml`定义加密变量

在向您的`.travis.yml`添加敏感数据（例如API凭据）之前，您需要对其进行加密。加密变量不适用于不受信任的构建，例如来自另一个存储库的拉取请求。

包含加密变量的`.travis.yml`文件如下所示：

```yaml
env:
  global:
    - secure: mcUCykGm4bUZ3CaW6AxrIMFzuAYjA98VIz6YmYTmM0/8sp/B/54JtQS/j0ehCD6B5BwyW6diVcaQA2c
  jobs:
    - USE_NETWORK=true
    - USE_NETWORK=false
    - secure: <你也可以把加密的变量放入此矩阵中>
```

> 加密的环境变量不可用于从fork拉取请求，因为存在将此类信息暴露给未知代码的安全风险。
>
> 如果在`.travis.yml`和存储库设置中定义了同名的变量，则`.travis.yml`中的变量优先。如果在`.travis.yml`中将变量定义为加密和未加密，则文件中稍后定义的变量优先。

#### 加密环境变量

使用`travis` gem将公钥附加到存储库来加密环境变量：

1. 如果未安装`travis` gem，请运行`gem install travis`（或macOS上的`brew install travis`）。

2. 在您的存储库目录中：

   * 如果您使用的是https://travis-ci.com，请参阅[加密密钥-用法](https://docs.travis-ci.com/user/encryption-keys#usage)。

   * 如果您使用的是https://travis-ci.org，请运行： 

     ```bash
       travis encrypt MY_SECRET_ENV=super_secret --add env.global
     ```

3. 提交更改到你的 `.travis.yml`.

> 加密和解密密钥与存储库绑定。如果派生一个项目并将其添加到Travis CI，则该项目将无权访问加密的变量。

加密方案在“ [加密密钥”](https://docs.travis-ci.com/user/encryption-keys)中有更详细的说明。 



### 在库设置定义变量

存储库设置中定义的变量对于所有构建都是相同的，并且当您重新启动旧构建时，它将使用最新值。这些变量不会自动用于派生。

在“存储库设置”中定义变量：

* 每个存储库都不同。
* 包含敏感数据，例如第三方凭据。



要在存储库设置中定义变量，请确保您已登录，导航到有问题的存储库，从 **更多选项**  菜单中选择  **设置** ，然后单击 **环境变量** 部分中的 **添加新变量** 。通过选择环境变量对哪个分支可用，将其限制为特定分支。


默认情况下，这些新环境变量的值在日志中的`export`行中隐藏。这对应于`.travis.yml`中[加密变量](https://docs.travis-ci.com/user/environment-variables/#Encrypted-Variables)的行为。 变量以加密方式存储在我们的系统中，并在生成构建脚本时解密。

同样，我们不向不受信任的构建提供这些值，这些构建是由来自另一个存储库的请求触发的。

作为Web界面的替代方法，您还可以使用CLI的[`env`](https://github.com/travis-ci/travis.rb#env)命令。 

> 如果在`.travis.yml`和存储库设置中定义了同名的变量，则`.travis.yml`中的变量优先。



### 默认环境变量

以下默认环境变量可用于所有构建。 

* `CI=true`
* `TRAVIS=true`
* `CONTINUOUS_INTEGRATION=true`
* `DEBIAN_FRONTEND=noninteractive`
* `HAS_JOSH_K_SEAL_OF_APPROVAL=true`
* `USER=travis`
* `HOME`   是设置为 `/home/travis` 在Linux， `/Users/travis` 在MacOS， `/c/Users/travis` 在Windows。
* `LANG=en_US.UTF-8`
* `LC_ALL=en_US.UTF-8`
* `RAILS_ENV=test`
* `RACK_ENV=test`
* `MERB_ENV=test`
* `JRUBY_OPTS="--server -Dcext.enabled=false -Xcompile.invokedynamic=false"`
* `JAVA_HOME` 设置为适当的值。 

另外，Travis CI设置了可在构建中使用的环境变量，例如，标记构建或运行构建后部署。 更多参阅 [原文](https://docs.travis-ci.com/user/environment-variables/#default-environment-variables)

* `TRAVIS_ALLOW_FAILURE`： 
  * 设置为`true`是允许作业失败。
  * 设置为`false`不允许作业失败。
* `TRAVIS_APP_HOST`：编译构建脚本的服务器的名称。此服务器提供某些辅助文件（例如`gimme`，`nvm`，`sbt`）从`/files`以避免外部网络的调用。 例如，`curl -O $TRAVIS_APP_HOST/files/gimme `

* `TRAVIS_BRANCH`： 
  * 对于推送构建，或者不是由拉取请求触发的构建，这是分支的名称。
  * 对于由拉取请求触发的构建，这是拉取请求所针对的分支的名称。
  * 对于由标签触发的构建，这与标签（`TRAVIS_TAG`）的名称相同。

* `TRAVIS_BUILD_DIR`：已在工作服务器上复制了要构建存储库的目录的绝对路径。
* `TRAVIS_BUILD_ID`：Travis CI内部使用的当前构建版本的ID。
* `TRAVIS_BUILD_NUMBER`：当前构建版本号（例如，“ 4”）。

* `TRAVIS_BUILD_WEB_URL`：构建日志的URL。

* `TRAVIS_OS_NAME`：在多操作系统构建中，此值指示作业在其上运行的平台。值目前`linux`，`osx`和`windows`（测试版），在将来进行扩展。

* `TRAVIS_CPU_ARCH`：在[多](https://docs.travis-ci.com/user/multi-cpu-architectures/)体系结构构建中，此值指示作业正在运行的CPU体系结构。值当前`amd64`，`arm64`，`ppc64le`和`s390x`。

* `TRAVIS_SUDO`：`true`或`false`基于是否启用`sudo`。

特定于语言的构建公开了其他环境变量，这些变量代表用于运行该构建的当前版本。是否设置它们取决于您使用的语言。

* `TRAVIS_DART_VERSION`
* `TRAVIS_GO_VERSION`
* `TRAVIS_HAXE_VERSION`
* `TRAVIS_JDK_VERSION`
* `TRAVIS_JULIA_VERSION`
* `TRAVIS_NODE_VERSION`
* `TRAVIS_OTP_RELEASE`
* `TRAVIS_PERL_VERSION`
* `TRAVIS_PHP_VERSION`
* `TRAVIS_PYTHON_VERSION`
* `TRAVIS_R_VERSION`
* `TRAVIS_RUBY_VERSION`
* `TRAVIS_RUST_VERSION`
* `TRAVIS_SCALA_VERSION`



## 配置构建通知

> 参阅： [配置构建通知](https://docs.travis-ci.com/user/notifications/)

Travis CI可以通过电子邮件，IRC，聊天或自定义Webhooks通知您有关构建结果的信息。 

> 请注意，如果您仍在使用[travis-ci.org](http://www.travis-ci.org/)，则需要使用`--org`而不是`--com`在此页面上显示的所有命令中使用。 



### 默认通知设置

默认情况下，当提交者和作者是存储库的成员时，会将电子邮件通知发送给他们

* 对公共存储库的推送或管理员权限。
* 私有存储库的拉，推或管理权限。

在给定的分支，电子邮件在以下情况下发送：

* 构建刚刚被破坏或仍然被破坏。
* 刚刚修复了以前损坏的构建。 



### 有条件的通知

您可以使用`if`来指定构建配置（您的`.travis.yml`文件）中的条件，从而过滤和拒绝通知。 

例如，这将仅在`master`分支的构建上发送[Slack通知](https://docs.travis-ci.com/user/notifications/#configuring-slack-notifications)： 

```yaml
# 要求分支名称master（注意，对于PRs，这是基本分支名称）
notifications:
  slack:
    if: branch = master
```

以下已知属性可用，并且可以在条件中使用：

* `type`（当前的事件类型，已知事件类型有：`push`，`pull_request`，`api`，`cron`）
* `repo`（当前存储库slug `owner_name/name`）
* `branch` （当前分支名称；对于拉取请求：基本分支名称）
* `tag` （当前标签名称）
* `commit_message` （当前的提交消息）
* `sender` （事件发送者的登录名）
* `fork`（`true`或`false`取决于存储库是否为fork）
* `head_repo`（对于拉取请求：头存储库slug `owner_name/name`）
* `head_branch` （对于拉取请求：头存储库分支名称）
* `os` （操作系统）
* `language` （构建语言）
* `sudo` （sudo访问）
* `dist` （分布）
* `group` （图像组）

此外，您的构建配置 (`.travis.yml`) 和存储库设置中的环境变量是可用的，并且可以使用 `env(FOO)` 进行匹配。

#### 布尔运算符

支持以下布尔运算符：

* `AND`
* `OR`
* `NOT`

`AND`结合强于`OR`，和`NOT`结合强于`AND`。因此，以下表达式相同：

```yaml
branch = master AND os = linux OR tag = bar
(branch = master AND os = linux) OR tag = bar

NOT branch = master AND os = linux
NOT (branch = master) AND os = linux
```

#### 值

值是不带引号的字符串，不包含任何空格或特殊字符，或单引号或双引号的字符串： 

```yaml
"a word"
'a word'
a_word
```

#### 函数调用

有两个函数可用：

* `env`
* `concat`

函数`env`返回给定环境变量的值：

```yaml
env(FOO)
```

`env`支持在您的`.travis.yml`中设置的`env`键（包括 `env.global`）或在存储库设置中指定的环境变量。 

函数`concat`接受任意数量的参数并将它们连接为单个字符串： 

```yaml
concat("foo", "-", env(BAR))
# => "foo-bar"
```

#### 列表

这与值列表（数组）匹配： 

```yaml
branch IN (master, dev)
env(foo) IN (bar, baz)
env(foo) IN ("bar baz", "buz bum")
```

请注意，必须使用逗号分隔值。包含空格或特殊字符的值应加引号 

运算符`IN`可以取反，如下所示： 

```yaml
# 这些都是一样的
NOT branch IN (master, dev)
branch NOT IN (master, dev)
```



更多有关指定条件的详细信息见 [条件](https://docs.travis-ci.com/user/conditions-v1)。 



### 更改通知频率

您可以通过将`on_success`或`on_failure`标记设置为以下一项来更改任何通知渠道的条件 ：

* `always`：始终发送通知。
* `never`：从不发送通知。
* `change`：当构建状态更改时发送通知。

例如，要始终在成功的构建上发送slack通知：

```yaml
notifications:
  slack:
    on_success: always
```

**注意：**这些Webhook是在构建结束时执行的，而不是由单个作业执行的（请参阅Build [vs](https://docs.travis-ci.com/user/for-beginners/#builds-jobs-stages-and-phases) Jobs ）。这意味着该版本中的构建环境变量不可用。 当前尚无法将通知限制为特定分支。



### 配置电子邮件通知

指定将收到构建结果通知的收件人： 

```yaml
notifications:
  email:
    - one@example.com
    - other@example.com
```

完全关闭电子邮件通知：

```yaml
notifications:
  email: false
```

指定何时要收到通知：

```yaml
notifications:
  email:
    recipients:
      - one@example.com
      - other@example.com
    on_success: never # 默认: change
    on_failure: always # 默认: always
```

拉取请求构建不会触发电子邮件通知。 

#### 如何确定构建电子邮件接收者？

默认情况下，构建电子邮件会发送给提交者和作者，但前提是他们有权访问提交被推送到的存储库。 

然后根据提交中的电子邮件地址确定电子邮件地址，但前提是该电子邮件地址与我们数据库中的电子邮件地址之一匹配。我们仅出于生成通知的目的同步来自GitHub的所有电子邮件地址。 

如上所示，可以覆盖`.travis.yml`默认值。如果指定了设置，Travis CI仅将电子邮件发送到此处指定的地址，而不发送给提交者和作者。 

#### 为构建通知更改的电子邮件地址

Travis CI仅向在GitHub上注册的电子邮件地址发送构建通知。如果您注册了多个地址，则可以使用以下`git`命令为特定存储库设置电子邮件地址：

> 请注意，这还会更改提交电子邮件地址，而不仅仅是Travis CI通知设置。

```Bash
git config user.email "mynewemail@example.com"
```

或为所有git存储库设置电子邮件：

```Bash
git config --global user.email "mynewemail@example.com"
```



## 安全最佳实践

> 参阅：[保护数据的最佳做法](https://docs.travis-ci.com/user/best-practices-security/)

### Travis CI保护数据的步骤 

Travis CI混淆了UI中显示的安全环境变量和令牌。我们[有关加密密钥的文档](https://docs.travis-ci.com/user/encryption-keys/)概述了确保这一点所需的构建配置，但是，一旦启动了VM并运行了测试，我们就无法控制哪些信息实用程序或附件可以打印到VM的标准输出。

为了防止这些组件泄漏，我们会在运行时自动过滤长度超过三个字符的安全环境变量和令牌，从而有效地将它们从构建日志中删除，而是显示字符串`[secure]`。

请确保您的机密绝不与存储库或分支名称或任何其他可猜测的字符串相关。理想情况下，使用密码生成工具（例如`mkpasswd`）来代替自己选择一个秘密。 

### 关于如何避免泄漏机密到构建日志的建议

尽管我们已尽了最大努力，但是有许多方法可以意外地暴露安全信息。这些会根据您使用的工具和启用的设置而有所不同。需要**注意的**一些事情是： 

* 将命令复制到标准输出的设置，例如bash脚本中的`set -x`或`set -v`
* 通过运行`env`或`printenv`显示环境变量
* 例如，在代码中打印秘密 `echo "$SECRET_KEY"`
* 使用在错误输出上打印机密的工具，例如 `php -i`
* git命令，例如`git fetch`或`git push`可能公开令牌或其他安全环境变量
* 字符串转义中的错误
* 增加命令详细程度的设置
* 在操作过程中可能会泄露秘密的测试工具或插件

防止命令显示任何输出是避免意外显示任何安全信息的一种方法。如果存在使用安全信息的特定命令，则可以将其输出重定向到`/dev/null`以确保它不会意外发布任何内容，如以下示例所示：

```
git push url-with-secret >/dev/null 2>&1
```

### 如果你认为你可能暴露了安全信息 

作为第一步，可以通过单击Travis CI的构建日志页面上的“*删除日志”*按钮来删除包含任何安全信息的*日志*。 

如果您在一个构建日志中发现泄漏，则必须撤销泄漏的令牌或环境变量，并更新导致泄漏的任何构建脚本或命令。 

### 定期轮换令牌和机密

定期轮换您的令牌和秘密。可以在GitHub站点上的[开发人员设置中](https://github.com/settings/developers)找到GitHub OAuth令牌。也请定期轮换其他第三方服务的凭据。 



## FAQ

### sudo

>  参阅：  [Travis CI构建配置参考](https://config.travis-ci.com/)  > [sudo](https://config.travis-ci.com/ref/sudo)

是否允许sudo访问

*不推荐使用：该键 sudo不再有效。*

```yaml
sudo: true
或
sudo: required
```





## 参考阅读

[Travis CI构建配置参考](https://config.travis-ci.com/) 

[构建 JavaScript 和Node.js 项目](https://docs.travis-ci.com/user/languages/javascript-with-nodejs/)

[使用travis-ci自动部署github上的项目](https://www.cnblogs.com/morang/p/7228488.html)



