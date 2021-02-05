---
title: GitHub Actions的工作流程命令
date: 2020-11-10  5:34:59
updated: 2020-11-10  5:34:59
mathjax: false
categories: 
tags:
typora-root-url: workflow-commands-for-github-actions
typora-copy-images-to: workflow-commands-for-github-actions
top: 1
comments: false
---


# GitHub Actions的工作流程命令

[原文](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-commands-for-github-actions)

> 在工作流程或动作代码中运行外壳程序命令时，可以使用工作流程命令。 



## 关于工作流程命令

动作可以与运行器机器通信以设置环境变量，其他动作使用的输出值，将调试消息添加到输出日志以及其他任务。

大多数工作流程命令以特定格式使用`echo`命令，而其他工作流程命令则通过写入文件来调用。有关更多信息，请参见[“环境文件”。](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-commands-for-github-actions#environment-files)

```yaml
echo "::workflow-command parameter1={data},parameter2={data}::{command value}"
```

> **注意：**工作流命令和参数名称不区分大小写。
>
> **警告：**如果使用的是命令提示符，则在使用工作流程命令时，请省略双引号字符（`"`）。

## 使用工作流命令访问工具箱函数

 [actions/toolkit](https://github.com/actions/toolkit) 包括许多可以作为工作流命令执行的函数。 使用`::`语法在YAML文件中运行工作流程命令；这些命令然后发送给运行器`stdout`。例如，代替使用代码来设置输出，如下所示：

```shell
core.setOutput('SELECTED_COLOR', 'green');
```

您可以在工作流程中使用`set-output`命令来设置相同的值：

```yaml
      - name: Set selected color
        run: echo '::set-output name=SELECTED_COLOR::green'
        id: random-color-generator
      - name: Get color
        run: echo "The selected color is ${{ steps.random-color-generator.outputs.SELECTED_COLOR }}"
```

下表显示了工作流程中可用的工具箱函数：

| 工具包函数            | 等效工作流程命令                      |
| --------------------- | ------------------------------------- |
| `core.addPath`        | 使用环境文件访问`GITHUB_PATH`         |
| `core.debug`          | `debug`                               |
| `core.error`          | `error`                               |
| `core.endGroup`       | `endgroup`                            |
| `core.exportVariable` | 使用环境文件可访问 `GITHUB_ENV`       |
| `core.getInput`       | 使用环境变量可访问 `INPUT_{NAME}`     |
| `core.getState`       | 使用环境变量可访问 `STATE_{NAME}`     |
| `core.isDebug`        | 使用环境变量可访问 `RUNNER_DEBUG`     |
| `core.saveState`      | `save-state`                          |
| `core.setFailed`      | 作为`::error`和`exit 1`的一个快捷方式 |
| `core.setOutput`      | `set-output`                          |
| `core.setSecret`      | `add-mask`                            |
| `core.startGroup`     | `group`                               |
| `core.warning`        | `warning file`                        |

## 设定输出参数

`::set-output name={name}::{value}`

设置动作的输出参数。

您也可以选择在动作的元数据文件中声明输出参数。有关更多信息，请参阅“ [GitHub Actions的元数据语法](https://docs.github.com/en/free-pro-team@latest/articles/metadata-syntax-for-github-actions#outputs)”。

**示例**

```yaml
echo "::set-output name=action_fruit::strawberry"
```

## 设置调试消息

`::debug::{message}`

将调试消息输出到日志。您必须创建一个名为`ACTIONS_STEP_DEBUG`且值为`true`的机密，才能在日志中查看此命令设置的调试消息。有关更多信息，请参阅“[启用调试日志记录”](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs/enabling-debug-logging)。

**示例**

```yaml
echo "::debug::Set the Octocat variable"
```

## 设置警告信息

`::warning file={name},line={line},col={col}::{message}`

创建警告消息并将该消息打印到日志。您可以选择提供发生警告的文件名（`file`），行号（`line`）和列（`col`）。

**示例**

```yaml
echo "::warning file=app.js,line=1,col=5::Missing semicolon"
```

## 设置错误信息

`::error file={name},line={line},col={col}::{message}`

创建一条错误消息并将该消息打印到日志中。您可以选择提供发生错误的文件名（`file`），行号（`line`）和列（`col`）。

**示例**

```yaml
echo "::error file=app.js,line=10,col=15::Something went wrong"
```

## 分组日志行

```yaml
::group::{title}
::endgroup::
```

在日志中创建一个可扩展的组。要创建组，请使用`group`命令并指定一个`title`。`group`和`endgroup`命令之间在日志中打印的所有内容都嵌套在日志中的可扩展条目内。

**示例**

```yaml
echo "::group::My title"
echo "Inside group"
echo "::endgroup::"
```

![工作流程运行日志中的可折叠组](/actions-log-group.png)

## 屏蔽日志中的值

`::add-mask::{value}`

屏蔽值可防止在日志中打印字符串或变量。用空格分隔的每个被屏蔽的单词都被替换为`*`字符。您可以为掩码的`value`使用环境变量或字符串。

### 屏蔽字符串的示例

在日志中打印`"Mona The Octocat"`时，您会看到`"***"`。

```yaml
echo "::add-mask::Mona The Octocat"
```

### 屏蔽环境变量的示例

在日志中打印变量`MY_NAME`或值`"Mona The Octocat"`时，您会看到`"***"`而不是`"Mona The Octocat"`。

```yaml
MY_NAME="Mona The Octocat"
echo "::add-mask::$MY_NAME"
```

## 停止和启动工作流程命令

`::stop-commands::{endtoken}`

停止处理任何工作流程命令。此特殊命令使您可以记录任何内容，而不会意外运行工作流命令。例如，您可以停止记录以输出带有注释的整个脚本。

### 停止工作流程命令的示例

```yaml
echo "::stop-commands::pause-logging"
```

要启动工作流程命令，请传递用于停止工作流程命令的令牌。

`::{endtoken}::`

### 启动工作流程命令的示例

```yaml
echo "::pause-logging::"
```

## 将值发送到动作前和动作后

您可以使用`save-state`命令创建环境变量以与您的工作流程`pre:`或`post:`动作共享。例如，您可以使用`pre:`动作创建文件，将文件位置传递给`main:`动作，然后使用`post:`动作删除文件。或者，您可以使用`main:`动作创建文件，将文件位置传递给`post:`动作，也可以使用`post:`动作删除文件。

如果有多个`pre:`或`post:`动作，您只能访问使用`save-state`的动作中保存的值。有关该`post:`动作的更多信息，请参阅“ [GitHub动作的元数据语法](https://docs.github.com/en/free-pro-team@latest/actions/creating-actions/metadata-syntax-for-github-actions#post)”。

`save-state` 命令只能在一个动作中运行，并且不适用于YAML文件。保存的值作为带有`STATE_`前缀的环境值存储。

本示例使用JavaScript运行`save-state`命令。结果环境变量的名称`STATE_processID`值为`12345`：

```javascript
console.log('::save-state name=processID::12345')
```

然后，`STATE_processID`变量仅可用于`main`动作下运行的清除脚本。本示例运行`main`并使用JavaScript显式分配给`STATE_processID`环境变量的值：

```javascript
console.log("The running PID from the main action is: " +  process.env.STATE_processID);
```



## 环境文件

在执行工作流期间，运行程序会生成可用于执行某些动作的临时文件。这些文件的路径通过环境变量公开。写入这些文件时，您将需要使用UTF-8编码，以确保正确处理命令。可以将多个命令写入同一文件，并以换行符分隔。

> **警告：**默认情况下，Powershell不使用UTF-8。确保使用正确的编码写入文件。例如，设置路径时需要设置UTF-8编码：
>
> ```yaml
> steps:
>   - run: echo "mypath" | Out-File -FilePath $env:GITHUB_PATH -Encoding utf8 -Append
> ```



## 设置环境变量

`echo "{name}={value}" >> $GITHUB_ENV`

为作业中接下来运行的任何动作创建或更新环境变量。创建或更新环境变量的动作无法访问新值，但是作业中的所有后续动作都可以访问。环境变量区分大小写，并且可以包含标点符号。

**示例**

```yaml
echo "action_state=yellow" >> $GITHUB_ENV
```

在以后的步骤中运行`$action_state`现在将返回`yellow`。

### 多行字符串

对于多行字符串，可以使用具有以下语法的定界符。

```yaml
{name}<<{delimiter}
{value}
{delimiter}
```

**示例**

在此示例中，我们用`EOF`作定界符，并将`JSON_RESPONSE`环境变量设置为curl响应的值。

```yaml
steps:
  - name: Set the value
    id: step_one
    run: |
        echo 'JSON_RESPONSE<<EOF' >> $GITHUB_ENV
        curl https://httpbin.org/json >> $GITHUB_ENV
        echo 'EOF' >> $GITHUB_ENV
```

## 添加系统路径

`echo "{path}" >> $GITHUB_PATH`

在系统`PATH`变量之前为当前作业中的所有后续动作添加目录。当前正在运行的动作无法访问新的路径变量。

**示例**

```yaml
echo "/path/to/dir" >> $GITHUB_PATH
```



## 参考阅读


