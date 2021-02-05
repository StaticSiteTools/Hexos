---
title: GitHub Actions的安全性强化
date: 2020-11-01 22:48:55
updated: 2020-11-01 22:48:55
mathjax: false
categories: 
tags:
typora-root-url: security-hardening-for-github-actions
typora-copy-images-to: security-hardening-for-github-actions
top: 1
comments: false
---


# GitHub Actions的安全性强化

[原文](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/security-hardening-for-github-actions)

> 使用GitHub Actions功能的良好安全做法。 



## 总览

本指南说明了如何为某些GitHub Actions功能配置安全性强化。如果GitHub Actions概念不熟悉，请参阅“ [GitHub Actions的核心概念](https://docs.github.com/en/free-pro-team@latest/actions/getting-started-with-github-actions/core-concepts-for-github-actions)”。

## 使用机密

敏感值绝不应以纯文本形式存储在工作流文件中，而应作为秘密存储。可以在组织或存储库级别配置[机密](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)，并允许您在GitHub中存储敏感信息。

秘密使用[Libsodium密封盒](https://libsodium.gitbook.io/doc/public-key_cryptography/sealed_boxes)，以便在到达GitHub之前对其进行加密。当使用[UI](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets#creating-encrypted-secrets-for-a-repository)或通过[REST API](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#secrets)提交机密时，就会发生这种情况。这种客户端加密有助于最大程度地降低与GitHub基础架构中意外记录（例如，异常日志和请求日志等）相关的风险。机密上传后，GitHub便可以对其解密，以便可以将其注入工作流运行时中。

为了帮助防止意外泄露，GitHub使用一种机制来尝试删除运行日志中出现的所有机密。此修订查找任何已配置机密的精确匹配，以及值的通用编码，例如Base64。但是，由于可以通过多种方式转换秘密值，因此无法保证此修改。因此，您应该遵循一些积极的步骤和良好做法，以帮助确保机密已被编辑，并限制与机密相关的其他风险：

* **永远不要将结构化数据用作秘密**
  * 非结构化数据可能导致日志中的秘密修改失败，因为修改很大程度上依赖于查找特定秘密值的精确匹配。例如，请勿使用JSON，XML或YAML（或类似名称）的blob封装机密值，因为这会大大降低机密被正确编辑的可能性。而是，为每个敏感值创建单独的机密。
* **注册工作流程中使用的所有机密**
  * 如果秘密用于在工作流中生成另一个敏感值，则该生成的值应正式[注册为secret](https://github.com/actions/toolkit/tree/main/packages/core#setting-a-secret)，以便一旦它出现在日志中就将被删除。例如，如果使用私钥生成签名的JWT以访问Web API，请确保将该JWT注册为机密，否则，一旦输入日志输出，就不会对其进行编辑。
  * 注册机密也适用于任何形式的转换/编码。如果您的机密以某种方式进行了转换（例如Base64或URL编码），请确保也将新值注册为机密。
* **审核机密的处理方式**
  * 审核机密的使用方式，以帮助确保按预期方式处理机密。您可以通过查看执行工作流的存储库的源代码，并检查工作流中使用的任何操作来做到这一点。例如，检查是否没有将它们发送到意外的主机，或明确地将其打印到日志输出。
  * 测试有效/无效输入后，查看工作流程的运行日志，并检查机密是否已正确编辑或未显示。您调用的命令或工具如何将错误发送到`STDOUT`和`STDERR`并不总是很明显，并且机密可能随后会出现在错误日志中。因此，优良作法是在测试有效和无效输入之后手动查看工作流日志。
* **使用范围最小的凭据**
  * 确保在工作流中使用的凭据具有所需的最少特权，并请注意，对存储库具有写访问权限的任何用户都对存储库中配置的所有机密都具有读访问权限。
* **审核和轮换注册机密**
  * 定期检查已注册的机密，以确认仍是必需的。删除不再需要的那些。
  * 定期轮换机密，以减少泄露机密有效的时间范围。

## 使用第三方动作

工作流中的各个作业可以与其他作业进行交互（并折衷）。例如，某作业查询后续作业使用的环境变量，将文件写入到稍后作业处理的共享目录中，或者通过与Docker套接字交互并检查其他正在运行的容器并在其中执行命令来直接将文件写入。

这意味着在工作流中对单个动作的折衷可能非常重要，因为该折衷的动作将可以访问您存储库中配置的所有机密，并可以使用`GITHUB_TOKEN`写入存储库。因此，从GitHub上的第三方存储库进行采购活动存在很大的风险。您可以通过遵循以下良好做法来帮助减轻这种风险：

* **将动作固定到全长提交SHA**

  将动作固定到全长提交SHA是当前将动作用作不可变版本的唯一方法。固定到特定的SHA有助于减轻不良行为者在动作的存储库中添加后门的风险，因为他们需要为有效的Git对象有效负载生成SHA-1冲突。

  **警告：**提交SHA的简短版本是不安全的，绝不能用于指定操作的Git参考。由于存储库网络是如何工作的，因此任何用户都可以派生存储库并向其中推送与短SHA冲突的精心设计的提交。这将导致该SHA上的后续克隆失败，因为它成为含糊的提交。结果，使用缩短的SHA的任何工作流程都将立即失败。

* **审核动作的源代码**

  确保该动作正在按预期方式处理存储库的内容和机密。例如，检查机密是否未发送到意外的主机，或者是否未在无意间记录。

* **仅当您信任创建者时，才能将动作固定到标签**

  尽管固定到提交SHA是最安全的选择，但是指定标签更加方便并且被广泛使用。如果您想指定标签，请确保您信任操作的创建者。GitHub Marketplace上的“已验证的创建者”徽章是一个有用的信号，因为它表明操作是由身份已由GitHub验证的团队编写的。请注意，即使您信任作者，此方法也存在风险，因为如果不良参与者获得对存储操作的存储库的访问权限，则可以移动或删除标签。

## 考虑跨存储库访问

有意将GitHub一次限定为一个存储库。工作流环境中使用的`GITHUB_TOKEN`授予与写访问用户相同的访问级别，因为任何写访问用户都可以通过创建或修改工作流文件来访问此令牌。

用户对每个存储库都具有特定的权限，因此，`GITHUB_TOKEN`如果不仔细实施，则为一个存储库授予对另一个存储库的访问权限将影响GitHub权限模型。同样，将GitHub身份验证令牌添加到工作流环境时必须小心，因为这也会通过无意中向协作者授予广泛访问权限而影响GitHub权限模型。

我们在[GitHub路线图](https://github.com/github/roadmap/issues/74)上有一个计划，以支持允许在GitHub内进行跨存储库访问的流程，但是尚不支持此功能。当前，执行特权跨存储库交互的唯一方法是在工作流环境中放置GitHub身份验证令牌或SSH密钥作为秘密。由于许多身份验证令牌类型不允许细粒度访问特定资源，因此使用错误的令牌类型存在很大的风险，因为它可以授予比预期范围宽得多的访问权限。

此列表按优先级降序介绍了在工作流中访问存储库数据的推荐方法：

1. **`GITHUB_TOKEN`工作流环境**
   * 该令牌有意地作用于调用该工作流的单个存储库，并且具有与该存储库上的写访问用户相同的访问级别。该令牌在每个作业开始之前创建，并在作业完成时过期。有关更多信息，请参见“[使用GITHUB_TOKEN进行身份验证](https://docs.github.com/en/free-pro-team@latest/actions/configuring-and-managing-workflows/authenticating-with-the-github_token)”。
   * 该`GITHUB_TOKEN`应尽可能使用。
2. **存储库部署密钥**
   * 部署密钥是授予单个存储库读写访问权限的唯一凭据类型之一，可用于与工作流程中的另一个存储库进行交互。有关更多信息，请参阅“[管理部署密钥”](https://docs.github.com/en/free-pro-team@latest/developers/overview/managing-deploy-keys#deploy-keys)。
   * 请注意，部署密钥只能使用Git克隆并推送到存储库，不能用于与REST或GraphQL API进行交互，因此它们可能不适合您的要求。
3. **GitHub应用令牌**
   * GitHub Apps可以安装在选定的存储库上，甚至可以对其中的资源具有精细的权限。您可以在组织内部创建GitHub App，将其安装在工作流中需要访问的存储库中，并通过工作流中的安装进行身份验证以访问这些存储库。
4. **个人访问令牌**
   * 您永远不要使用自己帐户中的个人访问令牌。这些令牌授予对您有权访问的组织内所有存储库以及用户帐户中所有个人存储库的访问权限。这间接地为工作流所在的存储库的所有写访问用户授予广泛访问权限。此外，如果您以后离开组织，使用此令牌的工作流将立即中断，调试此问题可能会很困难。
   * 如果使用个人访问令牌，则该令牌应该是为新帐户生成的令牌，该新帐户仅被授予对工作流程所需的特定存储库的访问权限。请注意，此方法不可扩展，应避免使用其他方法，例如部署密钥。
5. **用户帐户上的SSH密钥**
   * 工作流绝对不要在用户帐户上使用SSH密钥。与个人访问令牌类似，它们向您的所有个人存储库以及您可以通过组织成员身份访问的所有存储库授予读/写权限。这间接地为工作流所在的存储库的所有写访问用户授予了广泛的访问权限。如果您打算使用SSH密钥，因为您只需要执行存储库克隆或推送，而无需与公共API进行交互，则可以使用它，那么您应该改用单个部署密钥。

## 自托管运行程序的强化

**GitHub托管的运行程序**在临时且干净的隔离虚拟机中执行代码，这意味着没有办法永久破坏此环境，或者无法获得比引导过程中放置在此环境中更多的信息。

GitHub上的**自托管**运行程序无法保证在短暂的干净虚拟机中运行，并且可能会受到工作流中不受信任的代码的永久损害。

结果，几乎不应该将自托管的运行程序用于[GitHub上的公共存储库](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners#self-hosted-runner-security-with-public-repositories)，因为任何用户都可以针对存储库打开拉取请求并破坏环境。同样，在私有存储库上使用自托管运行器时要小心，因为任何能够派生存储库并打开PR的人（通常是那些具有存储库读取权限的人）都可以危害自托管运行器环境，包括获得访问权限机密和更高特权`GITHUB_TOKEN`，从而授予存储库上的写访问权限。

您还应该考虑自托管运行器计算机的环境：

* 配置为自托管运行程序的计算机上驻留哪些敏感信息？例如，私有SSH密钥，API访问令牌等。
* 机器是否可以通过网络访问敏感服务？例如，Azure或AWS元数据服务。在这种环境下，敏感信息的数量应保持在最低水平，并且您应始终牢记任何能够调用工作流的用户都可以访问此环境。

## 审核GitHub Actions事件

您可以使用审核日志来监视组织中的管理任务。审核日志记录操作的类型，运行时间以及执行该操作的用户帐户。

例如，您可以使用审核日志来跟踪`action:org.update_actions_secret`事件，该事件可以跟踪对组织机密的更改： ![审核日志条目](/audit-log-entries.png)

下表描述了您可以在审核日志中找到的GitHub Actions事件。有关使用审核日志的更多信息，请参阅“[查看组织的审核日志](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-organizations-and-teams/reviewing-the-audit-log-for-your-organization#searching-the-audit-log)”。

### 机密管理事件

| 行动                                | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| `action:org.create_actions_secret`  | 当组织管理员[创建GitHub Actions机密](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-an-organization)时触发。 |
| `action:org.remove_actions_secret`  | 当组织管理员删除GitHub Actions机密时触发。                   |
| `action:org.update_actions_secret`  | 当组织管理员更新GitHub Actions机密时触发。                   |
| `action:repo.create_actions_secret` | 当存储库管理员[创建GitHub Actions 机密](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository)时触发。 |
| `action:repo.remove_actions_secret` | 当存储库管理员删除GitHub Actions机密时触发。                 |
| `action:repo.update_actions_secret` | 当存储库管理员更新GitHub Actions机密时触发。                 |

### 自托管运行者事件

| 行动                                      | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| `action:org.register_self_hosted_runner`  | 当组织所有者[注册新的自托管运行者](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/adding-self-hosted-runners#adding-a-self-hosted-runner-to-an-organization)时触发。 |
| `action:org.remove_self_hosted_runner`    | 当组织所有者[删除自托管运行者](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/removing-self-hosted-runners#removing-a-runner-from-an-organization)时触发。 |
| `action:repo.register_self_hosted_runner` | 当存储库管理员[注册新的自托管运行器](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/adding-self-hosted-runners#adding-a-self-hosted-runner-to-a-repository)时触发。 |
| `action:repo.remove_self_hosted_runner`   | 当存储库管理员[删除自托管的运行器](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/removing-self-hosted-runners#removing-a-runner-from-a-repository)时触发。 |

### 自托管运行者群组事件

| 行动                                      | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| `action:org.runner_group_created`         | 在组织管理员[创建自托管运行者群组](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/managing-access-to-self-hosted-runners-using-groups#creating-a-self-hosted-runner-group-for-an-organization)时触发。 |
| `action:org.runner_group_removed`         | 当组织管理员删除自托管运行者群组时触发。                     |
| `action:org.runner_group_renamed`         | 当组织管理员重命名自托管的运行者群组时触发。                 |
| `action:org.runner_group_runners_added`   | 当组织管理员[将自托管运行者添加到组中](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/managing-access-to-self-hosted-runners-using-groups#moving-a-self-hosted-runner-to-a-group)时触发。 |
| `action:org.runner_group_runners_removed` | 当组织管理员从组中删除自托管运行者时触发。                   |



## 参考阅读


