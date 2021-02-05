---
title: GitHub托管的运行者规格
date: 2020-11-10  7:09:18
updated: 2020-11-10  7:09:18
mathjax: false
categories: 
tags:
typora-root-url: specifications-for-github-hosted-runners
typora-copy-images-to: specifications-for-github-hosted-runners
top: 1
comments: false
---


# GitHub托管的运行者规格

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/specifications-for-github-hosted-runners)

> GitHub提供了托管虚拟机来运行工作流。虚拟机包含可供GitHub Actions使用的工具，软件包和设置的环境。 



## 关于GitHub托管的运行者

GitHub托管的运行程序是由GitHub托管并安装了GitHub Actions运行程序应用程序的虚拟机。GitHub为运行Linux，Windows和macOS操作系统的用户提供帮助。

当您使用GitHub托管的运行程序时，机器维护和升级将由您来完成。您可以直接在虚拟机或Docker容器中运行工作流程。

您可以为工作流程中的每个作业指定运行者类型。工作流中的**每个作业都在虚拟机的新实例中执行**。作业中的所有步骤都在虚拟机的同一实例中执行，从而允许该作业中的动作使用文件系统共享信息。

GitHub Actions运行器应用程序是开源的。你可以作出贡献，并在文件问题在 [runner](https://github.com/actions/runner)  库。

### 面向GitHub托管的运行者的云主机

GitHub在Microsoft Azure的Standard_DS2_v2虚拟机上托管Linux和Windows运行程序，并安装了GitHub Actions运行程序应用程序。GitHub上托管的运行器应用程序是Azure Pipelines代理的分支。所有Azure虚拟机的入站ICMP数据包均被阻止，因此ping或traceroute命令可能不起作用。有关Standard_DS2_v2计算机资源的更多信息，请参见Microsoft Azure文档中的“ [Dv2和DSv2系列](https://docs.microsoft.com/azure/virtual-machines/dv2-dsv2-series#dsv2-series)”。

GitHub使用[MacStadium](https://www.macstadium.com/)托管macOS运行程序。

### GitHub托管的运行者的管理特权

Linux和macOS虚拟机均使用无密码运行`sudo`。当您需要执行命令或安装需要比当前用户更多特权的工具时，可以使用`sudo`而无需提供密码。有关更多信息，请参见“ [Sudo手册”](https://www.sudo.ws/man/1.8.27/sudo.man.html)。

Windows虚拟机被配置为以禁用了用户帐户控制（UAC）的管理员身份运行。有关更多信息，请参见Windows文档中的“[用户帐户控制的工作方式](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/how-user-account-control-works)”。

## 受支持的运行者和硬件资源

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

## 支持软件

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

## IP地址

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

## 文件系统

GitHub在虚拟机上的特定目录中执行动作和Shell命令。虚拟机上的文件路径不是静态的。使用环境变量的GitHub提供了构建文件路径为`home`，`workspace`和`workflow`目录。

| 目录                  | 环境变量            | 描述                                                         |
| --------------------- | ------------------- | ------------------------------------------------------------ |
| `home`                | `HOME`              | 包含与用户相关的数据。例如，此目录可能包含来自登录尝试的凭据。 |
| `workspace`           | `GITHUB_WORKSPACE`  | 动作和外壳命令在此目录中执行。一个动作可以修改此目录的内容，以后的动作可以访问该目录。 |
| `workflow/event.json` | `GITHUB_EVENT_PATH` | `POST`触发工作流的webhook事件的有效负载。每次执行动作以隔离动作之间的文件内容时，GitHub都会对此进行重写。 |

有关GitHub为每个工作流创建的环境变量的列表，请参阅“[使用环境变量”](https://docs.github.com/en/free-pro-team@latest/github/automating-your-workflow-with-github-actions/using-environment-variables)。

### Docker容器文件系统

在Docker容器中运行的动作在`/github`路径下具有静态目录。但是，我们强烈建议使用默认环境变量在Docker容器中构造文件路径。

GitHub保留`/github`路径前缀，并为动作创建三个目录。

* `/github/home`
* `/github/workspace`-**注意：** GitHub Actions必须由默认Docker用户（root）运行。确保您的Dockerfile没有设置`USER`指令，否则您将无法访问`GITHUB_WORKSPACE`。
* `/github/workflow`



## 参考阅读


