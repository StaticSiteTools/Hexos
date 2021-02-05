---
title: 加密机密
date: 2020-11-05 19:45:52
updated: 2020-11-05 19:45:52
mathjax: false
categories: 
tags:
typora-root-url: encrypted-secrets
typora-copy-images-to: encrypted-secrets
top: 1
comments: false
---


# 加密机密

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets)

> 加密的机密使您可以将敏感信息存储在存储库或组织中。 



## 关于加密机密

机密是您在存储库或组织中创建的加密环境变量。您创建的机密可在GitHub Actions工作流程中使用。GitHub使用[libsodium密封盒](https://libsodium.gitbook.io/doc/public-key_cryptography/sealed_boxes)来帮助确保机密在到达GitHub之前已加密，并且在您在工作流程中使用它们之前一直保持加密状态。

对于存储在组织级别的机密，您可以使用访问策略来控制哪些存储库可以使用组织机密。组织级机密使您可以在多个存储库之间共享机密，从而减少了创建重复机密的需要。在一个位置更新组织机密还可以确保更改在使用该机密的所有存储库工作流程中生效。

### 命名您的机密

以下规则适用于机密名称：

* 机密名称只能包含字母数字字符（`[a-z]`，`[A-Z]`，`[0-9]`）或下划线（`_`）。不允许使用空格。
* 机密名称不能以`GITHUB_`前缀开头。
* 机密名称不能以数字开头。
* 机密名称在创建它们的级别上必须是唯一的。例如，在组织级别创建的机密必须在该级别具有唯一的名称，而在存储库级别创建的机密在该存储库中必须具有唯一的名称。如果组织级别的机密与存储库级的机密名称相同，则存储库级的机密优先。

为了帮助确保GitHub修改日志中的机密信息，请避免将结构化数据用作机密信息的值。例如，避免创建包含JSON或编码的Git Blob的机密。

### 访问您的机密

要使机密可用于动作，必须在工作流文件中将机密设置为输入或环境变量。查看动作的README文件，以了解动作预期的输入和环境变量。有关更多信息，请参阅“ [GitHub Actions的工作流语法](https://docs.github.com/en/free-pro-team@latest/articles/workflow-syntax-for-github-actions/#jobsjob_idstepsenv)”。

如果您有权编辑文件，则可以使用和读取工作流文件中的加密机密。有关更多信息，请参阅“ [GitHub上的访问权限](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/access-permissions-on-github)”。

> **警告：** GitHub会自动编辑打印到日志的机密，但您应避免有意将机密打印到日志。

您还可以使用REST API管理机密。有关更多信息，请参见“[机密”](https://docs.github.com/en/free-pro-team@latest/v3/actions/secrets)。

### 限制凭证权限

生成凭据时，建议您授予尽可能少的权限。例如，不要使用个人凭据，而要使用[部署密钥](https://docs.github.com/en/free-pro-team@latest/v3/guides/managing-deploy-keys/#deploy-keys)或服务帐户。如果需要，请考虑授予只读权限，并尽可能限制访问。生成个人访问令牌（PAT）时，请选择最少的范围。



## 为存储库创建加密的机密

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



## 为组织创建加密的机密

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



## 审查对组织级机密的访问

您可以检查将哪些访问策略应用于组织中的机密。

1. 在GitHub上，导航到组织的主页。

2. 在您的组织名称下，点击 **设置**。

   ![img](/organization-settings-tab.png) 

3. 在左侧边栏中，点击**机密**。

4. 机密列表包括所有已配置的权限和策略。例如：

   ![img](/actions-org-secrets-list.png) 

5. 有关为每个机密配置的权限的更多详细信息，请单击**更新**。



## 在工作流程中使用加密的机密

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

### 使用Bash的示例

```yaml
steps:
  - shell: bash
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "$SUPER_SECRET"
```

### 使用PowerShell的示例

```yaml
steps:
  - shell: pwsh
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "$env:SUPER_SECRET"
```

### 使用Cmd.exe的示例

```yaml
steps:
  - shell: cmd
    env:
      SUPER_SECRET: ${{ secrets.SuperSecret }}
    run: |
      example-command "%SUPER_SECRET%"
```

## 机密限制

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





## 参考阅读


