---
title: TravisCI Development
date: 2020-10-23 23:45:56
updated: 2020-10-23 23:45:56
mathjax: false
categories: 
tags:
typora-root-url: TravisCI_Development
typora-copy-images-to: TravisCI_Development
top: 1
comments: false
---


# TravisCI Development

> 参阅：
>
> [TravisCI](TravisCI.md)
>
> [Travis CI构建配置参考](https://config.travis-ci.com/) 
>
> [构建 JavaScript 和Node.js 项目](https://docs.travis-ci.com/user/languages/javascript-with-nodejs/)
>
> [使用travis-ci自动部署github上的项目](https://www.cnblogs.com/morang/p/7228488.html)



**项目源**和**推送目标**，不推荐在同一个仓库。完整内容在`.travis.yml`文件中。



## 基础配置

### 语言与版本

```yaml
# 语言
language: node_js
# 语言版本(node:最新稳定版 | lts/*:最新LTS版本)
node_js:
  - node
```

版本可指定具体的数字。



### 缓存

```yaml
# 缓存
# 缓存不经常更改的内容，加快构建速度。
cache:
    apt: true
    npm: true
    #目录
    directories:
        - node_modules 
```



### 环境变量

```yaml
# 环境变量
env:
 global:  
   - HEXOS_U_NAME: XXX
   - HEXOS_U_EMAIL: XXX@163.com
   - HEXOS_GITHUB_REF: github.com/XX/XX.github.io.git    
   - HEXOS_GITHUB_BRANCH: master
   - HEXOS_GITEE_REF: gitee.com/XX/XX.gitee.io.git
   - HEXOS_GITEE_BRANCH: master
   - HEXOS_CODING_REF: e.coding.net/XX/XX.coding.io.git
   - HEXOS_CODING_BRANCH: master
   - HEXOS_NOTICE_EMAIL: XXX@163.com
   - TZ: 'Asia/Shanghai'
   #- HEXOS_WATCH_BRANCH: master
```

注意，这些环境变量推荐在TravisCI中的库设置中定义。

   - `HEXOS_U_NAME`:  提交者名称。
   - `HEXOS_U_EMAIL`:  提交者邮件地址。
   - `HEXOS_GITHUB_REF`:  仓库地址，[Github](https://github.com/)。无`https`。
   - `HEXOS_GITHUB_BRANCH`:  远端分支，[Github](https://github.com/)。
   - `HEXOS_GITEE_REF`:  仓库地址，[Gitee](https://gitee.com/)。无`https`。
   - `HEXOS_GITEE_BRANCH`: 远端分支，[Gitee](https://gitee.com/)。
   - `HEXOS_CODING_REF`:  仓库地址，[Coding](https://coding.net/)。无`https`。
   - `HEXOS_CODING_BRANCH`: 远端分支，[Coding](https://coding.net/)。
   - `HEXOS_NOTICE_EMAIL`:  接收通知的邮件地址。
   - `TZ`:  时区。参阅  [时区列表](https://www.php.net/manual/zh/timezones.php)。**必需**
   - ~~`HEXOS_WATCH_BRANCH` :  监控项目源中的分支。~~
        - 经过实践，`branches`节点下好像不能使用变量。



### 分支监控

```yaml
# 分支监控
# 要运行您的构建的分支。
branches:
  only:
    - master
```

此分支是 项目源 的仓库分支，通常是`.travis.yml`文件所在分支。

只有指定的分支有提交时才会运行构建。

经过实践，好像不能使用变量，因此，如下不能运行：

```yaml
# 分支监控
# 要运行您的构建的分支。
branches:
  only:
    - ${HEXOS_WATCH_BRANCH}  
```



## 在安装之前

```yaml
before_install:
    - export TZ="${TZ}" #更改时区
```



## 安装

```yaml
install:
  # 安装依赖
  - ./Start  install
  # 补丁
  - ./Start  patch
```

通常情况下，只需如下内容

```yaml
install:
  - npm install  #安装依赖，需要package.json文件
```



## 脚本

```yaml
script:
  - hexo clean            #清除
  - hexo generate         #生成
```

可选

```yaml
script:
  - hexo clean            #清除
  - hexo bangumi -d       #删除数据
  - hexo bangumi -u       #更新数据  
  - hexo generate         #生成
```

残留内容

```yaml
script:
  - chmod 777 ./node_modules/.bin/hexo       #修改权限
  - chmod 777 ./node_modules/.bin/gulp       #修改权限
  - hexo clean                               #清除
  #- hexo g && gulp                           #生成(需要安装gulp插件)
  - hexo g                                   #生成
```



## 在脚本之后

### public处理

#### 方案1: 保留master分支的提交记录

```yaml
after_script:
  ## 方案1: 保留master分支的提交记录 ##  
  #------------START: git数据库获取 ------------#
  ## 克隆并检出分支，然后复制 .git 数据库到指定目录
  - git clone https://${HEXOS_GITHUB_REF} .deploy_git  
  - cd .deploy_git
  - git checkout master
  - cd ../
  - mv .deploy_git/.git/ ./public/ 
  #-------------END: git数据库获取 -------------#
  
  #------------START: public处理 ------------#
  ## public 目录已是git仓库，且保留了以往的提交记录，可根据实际需求检出文件
  - cd ./public
  # - git checkout .gitignore                   # 可选
  # - git checkout README.md                    # 可选  
  # - git checkout CNAME                        # 可选  
  - git config user.name "${HEXOS_U_NAME}"      # 配置name
  - git config user.email "${HEXOS_U_EMAIL}"    # 配置email
  - git config http.postBuffer  524288000       # 配置提交缓存
  - git add .
  - git commit -m "Travis CI 自动构建在 `date +"%Y-%m-%d %H:%M"`"  
  #-------------END: public处理 -------------#    
```

#### 方案2: 不保留master分支的提交记录

当前采用此方案。

```yaml
after_script:
  ## 方案2: 不保留master分支的提交记录 ##
  # 为达预期效果，不能有残留的旧文件，所以仓库每次都是1个commit  
  
  #------------START: public处理 ------------#
  - cd ./public
  - git init
  - git config user.name "${HEXOS_U_NAME}"   # 配置name
  - git config user.email "${HEXOS_U_EMAIL}" # 配置email
  - git config http.postBuffer  524288000    # 配置提交缓存
  - git add .
  - git commit -m "Travis CI 自动构建在 `date +"%Y-%m-%d %H:%M"`"  
  #-------------END: public处理 -------------#  
```



### 推送至远端仓库

```yaml
after_script: 
  ... ...    
  #------------START: 推送至远端仓库 ------------#  
  # Github 
  - git push --force --quiet "https://${HEXOS_GITHUB_TOKEN}@${HEXOS_GITHUB_REF}" master:${HEXOS_GITHUB_BRANCH}
  
  # Gitee
  #- git push --force --quiet "https://${HEXOS_GITEE_USER}:${HEXOS_GITEE_TOKEN}@${HEXOS_GITEE_REF}" master:${HEXOS_GITEE_BRANCH}
  
  # Coding
  #- git push --force --quiet "https://${HEXOS_CODING_PROJECT_USER}:${HEXOS_CODING_PROJECT_TOKEN}@${HEXOS_CODING_REF}" master:${HEXOS_CODING_BRANCH} 
  #-------------END: 推送至远端仓库 -------------#
```

注意： `*_TOKEN` 变量和`*_USER`变量必须配置在TravisCI中的库设置中。



## 通知配置

```yaml
notifications:
  email:
    recipients:  
      - ${HEXOS_NOTICE_EMAIL}
    on_success: never
    on_failure: always
```

`on_success ` ：在成功时；`on_failure`：在失败时。都具有如下可用值：

* `always`：始终发送通知。
* `never`：从不发送通知。
* `change`：当构建状态更改时发送通知。





## 参考阅读


