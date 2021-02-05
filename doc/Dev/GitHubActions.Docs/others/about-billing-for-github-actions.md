---
title: 关于GitHub动作的计费
date: 2020-11-10 22:21:08
updated: 2020-11-10 22:21:08
mathjax: false
categories: 
tags:
typora-root-url: about-billing-for-github-actions
typora-copy-images-to: about-billing-for-github-actions
top: 1
comments: false
---


# 关于GitHub动作的计费

[原文](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions)

> 如果您想在帐户中包含的存储空间或分钟数之外使用GitHub Actions，则会向您收取其他使用费用。 



## 关于GitHub动作的计费

**公共存储库和自托管运行者免费使用GitHub Actions**。对于私有存储库，每个GitHub帐户都会获得一定数量的空闲时间和存储空间，具体取决于该帐户使用的产品。默认情况下，您的帐户的支出限制为$0 ，这会在达到这些限制后阻止分钟或存储空间的额外使用。如果您将支出限额提高到默认值$0 以上，则超出该限额（也称为超额）的任何分钟或存储都将向您收费。GitHub将使用费支付给拥有工作流所在存储库的帐户。您帐户上的任何优惠券都不适用于GitHub Actions超额费用。

分钟会每月重置一次，而存储空间则不会重置。

| 产品                   | 存储    | 分钟（每月） |
| ---------------------- | ------- | ------------ |
| GitHub免费             | 500  MB | 2,000        |
| GitHub专业版           | 1 GB    | 3,000        |
| 适用于组织的GitHub免费 | 500  MB | 2,000        |
| GitHub团队             | 2 GB    | 3,000        |
| GitHub企业云           | 50 GB   | 50,000       |

在GitHub托管的Windows和macOS运行程序上运行的作业消耗的分钟数是Linux运行程序上的作业消耗速度的2到10倍。例如，使用1,000分钟的Windows分钟将消耗您帐户中所包含的2000分钟。使用1,000 macOS分钟，您的帐户中将消耗10,000分钟。

| 操作系统 | 分钟乘数 |
| -------- | -------- |
| Linux    | 1        |
| macOS    | 10       |
| Windows  | 2        |

存储库使用的存储是GitHub Actions工件和GitHub软件包使用的总存储。您的存储费用是您帐户拥有的所有存储库的总使用量。有关GitHub软件包定价的更多信息，请参阅“[关于GitHub软件包计费](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-packages)”。

如果您的帐户使用量超过了这些限制，并且您将支出限制设置为$0以上，则您将按GitHub托管运行器所使用的操作系统，按月每分钟每GB的存储量支付$ 0.25 USD。GitHub将每个作业使用的分钟取整到最接近的分钟。

> **注意：**分钟乘数不适用于以下所示的每分钟费率。

| 操作系统 | 每分钟费率 |
| -------- | ---------- |
| Linux    | $0.008     |
| macOS    | $0.08      |
| Windows  | $0.016     |

您可以在用户或组织帐户中的所有存储库中同时运行的作业数量取决于GitHub计划。有关更多信息，请参阅GitHub托管的运行器的“[使用限制和计费](https://docs.github.com/en/free-pro-team@latest/actions/reference/usage-limits-billing-and-administration)”，以及自托管的运行器使用限制的“[关于自托管的运行器](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners/#usage-limits)”。

## 计算分钟和存储支出

到月底，GitHub会根据您帐户中包含的数量来计算分钟和存储空间的成本。例如，如果您的组织使用GitHub 团队，并允许无限制的支出，则使用15,000分钟可能会有56美元的总存储空间和分钟超额成本，这取决于用于运行作业的操作系统。

* 5,000分钟（3,000 Linux和2,000 Windows）= $56（$24 + $32）。
  * 3,000个Linux分钟，每分钟$ 0.008 = $24。
  * 2,000个Windows分钟，每分钟$ 0.016 = $32。

在月底，GitHub将您的数据传输四舍五入到最接近的GB。

GitHub根据当月的每小时使用量来计算您每个月的存储使用量。例如，如果您在3月的10天中使用3 GB的存储空间，在3月的21天中使用12 GB的存储空间，则存储使用量将为：

* 3 GB x 10天x（每天24小时）= 720 GB-小时
* 12 GB x 21天x（每天24小时）= 6,048 GB-小时
* 720 GB-小时+ 6,048 GB-小时= 6,768 GB-小时
* 6,768 GB /小时/（每月744小时）= 9.0967 GB /月

在月底，GitHub将您的存储空间四舍五入到最近的MB。因此，您三月份的存储使用量为9.097 GB。

您的GitHub Actions使用情况会共享您帐户的现有账单日期，付款方式和收据。要查看您的GitHub帐户的所有订阅，请参阅“[查看您的订阅和账单日期”](https://docs.github.com/en/free-pro-team@latest/articles/viewing-your-subscriptions-and-billing-date)。

## 关于支出限额

默认情况下，您的帐户的GitHub Actions使用限制为$0。要使私人存储库的分钟数和存储空间超出您帐户所包含的数量，您可以增加支出限额或允许无限制支出。有关更多信息，请参阅“[管理GitHub Actions的支出限制](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/managing-your-spending-limit-for-github-actions)”。

如果您通过发票为企业帐户付款，则无法在GitHub上管理企业帐户的支出限额。如果您希望允许企业帐户拥有的组织在其帐户中包含的存储或数据传输之外使用GitHub Action，则可以预付超额费用。由于必须预付超额费用，因此您无法在以发票支付的帐户上实现无限支出。您的支出限额为预付款金额的150％。如有任何疑问，请[联系我们的客户管理团队](https://enterprise.github.com/contact)。

如果您的帐户有未付的未付费用：

* 在成功处理付款之前，不会重置您帐户中包含GitHub Actions和GitHub Packages的存储空间或分钟数。
* 对于在当前计费期剩余存储量或分钟数的帐户，GitHub Actions和GitHub Packages将继续可用，直到达到包括的使用量为止。
* 对于在GitHub Actions或GitHub Packages的当前计费期已达到使用量的帐户，将禁用GitHub Actions和GitHub Packages，以防止进一步超支。如果您通过发票付款，则必须[与我们的帐户管理团队联系](https://enterprise.github.com/contact)以处理付款并重置您的使用方式。





## 参考阅读


