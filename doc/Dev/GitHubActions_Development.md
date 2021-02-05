---
title: GitHubActions Development
date: 2020-11-16 15:08:58
updated: 2020-11-16 15:08:58
mathjax: false
categories: 
tags:
typora-root-url: GitHubActions_Development
typora-copy-images-to: GitHubActions_Development
top: 1
comments: false
---


# GitHubActions Development

> 参阅
>
> [使用GitHub Actions自动部署Hexo博客到GitHub Pages](https://zhuanlan.zhihu.com/p/161969042)



**项目源**和**推送目标**，不推荐在同一个仓库。完整内容在`.github/workflows/GitHubActions.yml`文件中。



## 触发条件

```yaml
# 触发条件：在 push 到 master 分支后
on:
  push:
    branches: 
      - master
```



## 环境变量

```yaml
env:
  TZ: Asia/Shanghai
  HEXOS_U_NAME: XXX
  HEXOS_U_EMAIL: XXX@163.com
  HEXOS_GITHUB_REF: github.com/XX/XX.github.io.git    
  HEXOS_GITHUB_BRANCH: master
  HEXOS_GITEE_REF: gitee.com/XX/XX.gitee.io.git
  HEXOS_GITEE_BRANCH: master
  HEXOS_CODING_REF: e.coding.net/XX/XX.coding.io.git
  HEXOS_CODING_BRANCH: master
  #HEXOS_NOTICE_EMAIL: XXX@163.com  
  #HEXOS_WATCH_BRANCH: master
```

注意，这些环境变量推荐在库设置中定义。

* `TZ`:  时区。参阅  [时区列表](https://www.php.net/manual/zh/timezones.php)。 **必需**
* `HEXOS_U_NAME`:  提交者名称。
* `HEXOS_U_EMAIL`:  提交者邮件地址。
* `HEXOS_GITHUB_REF`:  仓库地址，[Github](https://github.com/)。无`https`。
* `HEXOS_GITHUB_BRANCH`:  远端分支，[Github](https://github.com/)。
* `HEXOS_GITEE_REF`:  仓库地址，[Gitee](https://gitee.com/)。无`https`。
* `HEXOS_GITEE_BRANCH`: 远端分支，[Gitee](https://gitee.com/)。
* `HEXOS_CODING_REF`:  仓库地址，[Coding](https://coding.net/)。无`https`。
* `HEXOS_CODING_BRANCH`: 远端分支，[Coding](https://coding.net/)。
* ~~`HEXOS_NOTICE_EMAIL`:  接收通知的邮件地址。~~
* ~~`HEXOS_WATCH_BRANCH` :  监控项目源中的分支。~~



##作业

### 基础

```yaml
jobs:
  Hexos-cicd:
    name: Hexos 构建和部署
    runs-on: ubuntu-latest
```

使用最新的 Ubuntu 系统作为编译部署的环境



### 步骤: 检出代码

```yaml
    steps:
    - name: 检出代码
      uses: actions/checkout@v2
      with:
        #是否下载Git-LFS文件(true|false)
        lfs: 'true'
        #是否检出子模块(true:检出|recursive:递归检出)
        submodules: 'true'
```

这里使用 [actions/checkout](actions/checkout)  动作。

此动作将检出你的存储库到 `$GITHUB_WORKSPACE` 目录下。

身份验证令牌保留在本地git配置中。这使您的脚本可以运行经过身份验证的git命令。在工作后清理期间将删除令牌。 

未指定的参数，均采用默认值。如

* `repository`  :  当前仓库。
* `ref`  :  默认分支，通常为`master`

更多的参数，参阅  [actions/checkout](actions/checkout)  



### 步骤: 安装Node.js环境

```yaml
    - name: 安装Node.js环境
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'
```

采用 [actions/setup-node](actions/setup-node) 动作



### 步骤: 缓存Node.js模块

设置包缓存目录，避免每次下载

```yaml
    - name: 缓存Node.js模块
      uses: actions/cache@v2
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-node-
```

采用 [actions/cache](actions/cache )  动作。

此动作允许缓存依赖项并构建输出以缩短工作流执行时间。 更多参阅[“缓存依赖项以加快工作流程”](https://help.github.com/github/automating-your-workflow-with-github-actions/caching-dependencies-to-speed-up-workflows)。 

`cache` 动作将尝试根据您提供的`key` 还原缓存。当动作找到缓存时，该动作将缓存的文件恢复到配置的`path`。
* `path` :  运行器上要缓存或还原的文件路径。
* `key` :  保存高速缓存时创建的键和用于搜索高速缓存的键。 
* `restore-keys` : 如果未发生缓存命中，则用于查找缓存的备用键 。

**限制**

一个存储库最多可以有5GB的缓存。达到5GB限制后，将根据上次访问缓存的时间驱逐较早的缓存。上周未访问的缓存也将被驱逐。 

> [关于缓存工作流程依赖项](https://docs.github.com/en/free-pro-team@latest/actions/guides/caching-dependencies-to-speed-up-workflows#about-caching-workflow-dependencies) ：
>
> “在GitHub上托管的运行程序上的作业(`jobs`)在一个干净的虚拟环境中启动，并且每次必须下载依赖项”
>
> 当前，我们的文件中只有一个作业(`jobs`)，因此，作用并不大。



### 步骤: 安装Hexo

```yaml
      # 下载 hexo-cli 脚手架及相关安装包
    - name: 安装Hexo
      run: |
        npm install -g hexo-cli
        chmod u+x Start
        ./Start  install
        ./Start  patch
```



### 步骤: 生成文件

```yaml
      # 编译 markdown 文件
    - name: 生成文件
      run: |
        hexo clean
        hexo generate
```



### 步骤: Public存储库预处理

```yaml
      # Public存储库预处理
    - name: 初始化Public存储库   
      run: |        
        cp -f .gitattributes  public/
        cd ./public && git init && git lfs install
        git config lfs.allowincompletepush true
        git config user.name "${HEXOS_U_NAME}"
        git config user.email "${HEXOS_U_EMAIL}"
        git config http.postBuffer  524288000
        git add .
        git commit -m "GitHub Actions 自动构建在 `date +"%Y-%m-%d %H:%M"`"
```

当前采用不保留分支的提交记录，所以仓库每次都是1个commit。其中如下代码是处理大文件的。

```yaml
... ...      
        cp -f .gitattributes  public/
        ... ... && git lfs install
        git config lfs.allowincompletepush true
        ... ...
```

**注**：各平台大文件支持情况

* Github： 支持，有一定的免费额度量。
* Gitee： 只有企业版支持。
* Coding：支持，暂未收费。



### 步骤: 部署Hexos

```yaml
      # 将编译后的文件推送到指定仓库
    - name: 部署Hexos到GITHUB
      continue-on-error: true
      run: |  
        cd ./public
        git push --force --quiet "https://${{ secrets.HEXOS_GITHUB_TOKEN }}@${HEXOS_GITHUB_REF}" master:${HEXOS_GITHUB_BRANCH}
```

也可以推送到多个仓库，只需去掉前面的注释符号`#`。

```yaml
      # 将编译后的文件推送到指定仓库
    - name: 部署Hexos到GITHUB
      continue-on-error: true
      run: |  
        cd ./public
        git push --force --quiet "https://${{ secrets.HEXOS_GITHUB_TOKEN }}@${HEXOS_GITHUB_REF}" master:${HEXOS_GITHUB_BRANCH}
    - name: 部署Hexos到CODING
      continue-on-error: true
      run: |  
        cd ./public
        git push --force --quiet "https://${{ secrets.HEXOS_CODING_PROJECT_USER }}:${{ secrets.HEXOS_CODING_PROJECT_TOKEN }}@${HEXOS_CODING_REF}" master:${HEXOS_CODING_BRANCH}  
    - name: 部署Hexos到GITEE
      continue-on-error: true
      run: |  
#         rm -rf public/.git
#         rm -f public/.gitattributes
#         git init public/
#         git config -f public/.git/config  user.name "${HEXOS_U_NAME}"
#         git config -f public/.git/config  user.email "${HEXOS_U_EMAIL}"
#         git config -f public/.git/config  http.postBuffer  524288000
#         ( cd ./public && git add . && git commit -m "GitHub Actions 自动构建在 `date +"%Y-%m-%d %H:%M"`" )
        cd ./public
        git push --force --quiet "https://${{ secrets.HEXOS_GITEE_USER }}:${{ secrets.HEXOS_GITEE_TOKEN }}@${HEXOS_GITEE_REF}" master:${HEXOS_GITEE_BRANCH}
```

Gitee平台的LFS支持只限企业版，如果没有购买企业版的话，可启用上面注释的代码。

一些变量设置在平台中。

* `secrets.*_USER`
* `secrets.*_TOKEN`

注意： 读取平台中的变量的格式为`${{ secrets.XXX }}`



## 参考阅读

[使用GitHub Actions自动部署Hexo博客到GitHub Pages](https://zhuanlan.zhihu.com/p/161969042)



[Hexo+GitHub Actions 完美打造个人博客](https://www.cnblogs.com/i-code/p/12869046.html)

[GitHub Action + Hexo实现在线写作](https://blog.csdn.net/qq_41426117/article/details/108703295)

[利用GitHub+Actions自动部署Hexo博客](https://blog.csdn.net/u012208219/article/details/106883054)

[Hexo 使用 Github Actions 自动更新](https://www.jianshu.com/p/7940fe40885d)