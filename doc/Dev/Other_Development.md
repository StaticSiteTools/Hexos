---
title: Other Development
date: 2021-01-11 22:34:26
updated: 2021-01-11 22:34:26
mathjax: false
categories: 
tags:
typora-root-url: Other_Development
typora-copy-images-to: Other_Development
top: 1
comments: false
---


# Other Development

原文



## 身份验证

各代码托管平台的身份验证都类似，有各种各样的。

按类型划分

* **用户名和密码**
* **个人访问令牌**
  * 代替密码，通常只能用于HTTPS Git操作。
  * 即，在平台上设置相关权限，然后生成哈希字符串，用此字符串代替密码。
* **密钥**
  * 无需在每次访问时都提供用户名和个人访问令牌。
  * 生成公钥和私钥，公钥添加到平台中，私钥留在本地计算机中。

按规模可分为

* **个人账户级别**
  * 用户名和密码，个人访问令牌，密钥等形式都有。
* **储存库级别**
  * 针对单个存储库的读写权限，有些平台只有读的权限。
  * 实现形式通常是用密钥。



### Github

**[GitHub](https://github.com)提供的身份验证方式主要有：**

* 具有双因素身份验证的用户名和密码
* 个人访问令牌
  * 代替密码。—— [创建个人访问令牌](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token) 
  * 个人访问令牌只能用于HTTPS Git操作。
  * 格式： `https://$GH_TOKEN@github.com/owner/repo.git `
* SSH密钥
  * 无需在每次访问时都提供用户名和个人访问令牌。  —— [关于SSH](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/about-ssh)
  * 生成公钥和私钥，公钥添加到GitHub账户中，私钥留在本地计算机中。——  [使用SSH连接到GitHub](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/connecting-to-github-with-ssh)

**自动化部署时，身份验证方式也类似**

* 使用OAuth令牌进行HTTPS克隆
  * 即 使用个人访问令牌。
* **部署密钥**
  * 使用SSH密钥，仅将访问权限授予单个存储库。只读或读写权限。
  * [管理部署密钥](https://docs.github.com/en/free-pro-team@latest/developers/overview/managing-deploy-keys)
  * [部署密钥](https://docs.github.com/en/free-pro-team@latest/developers/overview/managing-deploy-keys#deploy-keys)
* 机器用户
  * 创建一个新的GitHub帐户，并附加一个专用于自动化的SSH密钥。



### Gitee

**[Gitee](https://gitee.com/)提供的身份验证方式主要有：**

* 用户名和密码
* 个人访问令牌
  * 代替密码。—— [创建个人访问令牌](https://gitee.com/profile/personal_access_tokens) 
  * [格式](https://gitee.com/oschina/git-osc/issues/IF6G5)： `https://user:token@gitee.com/user/myproject.git`
    * `user`： 个人空间地址后缀，如`https://gitee.com/XXX` 中的 `XXX`。
* SSH密钥
  * 无需在每次访问时都提供用户名和个人访问令牌。  —— [SSH公钥](https://gitee.com/profile/sshkeys)
  * 生成公钥和私钥，公钥添加到GitHub账户中，私钥留在本地计算机中。—— [SSH 公钥设置](https://gitee.com/help/articles/4191)

**自动化部署时，身份验证方式也类似**

* 个人访问令牌
* 账户密钥
  * 即上述的账户级别的SSH密钥，具有读写权限。
* **部署密钥**
  * 使用SSH密钥，仅将访问权限授予单个存储库。当前为只读权限。
  * [公钥管理](https://gitee.com/help/categories/38) 



### Coding

**[Coding](https://coding.net/)提供的身份验证方式主要有：**

* 具有双因素身份验证的用户名和密码
* 个人访问令牌
  * 代替用户名密码 。—— [创建个人访问令牌](https://help.coding.net/docs/member/tokens.html) 
  * 个人访问令牌只能用于HTTPS Git操作。
  * [格式](https://help.coding.net/docs/member/tokens.html)： `https://TokenUser:Token@test.coding.net/test/testRepo.git`
* SSH密钥
  * 无需在每次访问时都提供用户名和个人访问令牌。 
  * 生成公钥和私钥，公钥添加到GitHub账户中，私钥留在本地计算机中。
  * 一个 SSH 公钥文件，如果和 CODING 账户关联，便称为**账户 SSH 公钥，配置后拥有账户下所有项目的读写权限**；如果和某一个项目关联，则称为**部署公钥，配置后默认拥有该项目的只读权限。**  —— [配置 SSH 公钥](https://help.coding.net/docs/project-settings/features/ssh.html)
    * **部署公钥**，在项目的代码仓库的设置中配置。

**自动化部署时，身份验证方式也类似**

* 个人访问令牌

* [项目令牌](https://help.coding.net/docs/project-settings/features/deploy-tokens.html)

  * 在项目设置中配置。
  * 格式： `https://ProjectTokenUser:ProjectToken@test.coding.net/test/testRepo.git`

* **部署密钥**

  * 使用SSH密钥，仅将访问权限授予单个存储库。只读或读写权限。
  * 一个 SSH 公钥文件，如果和 CODING 账户关联，便称为**账户 SSH 公钥，配置后拥有账户下所有项目的读写权限**；如果和某一个项目关联，则称为**部署公钥，配置后默认拥有该项目的只读权限。** 
    * **部署公钥**，在项目的代码仓库的设置中配置。
  * [配置 SSH 公钥](https://help.coding.net/docs/project-settings/features/ssh.html)  

  

## 方案采用

基于权限范围考虑，优先使用范围较小的仓库级别的 **部署密钥**，如果它具有读写权限的话。

其次，考虑，**个人访问令牌**。



### 密钥使用

[ssh密钥使用参考示例](https://blog.csdn.net/u012208219/article/details/106883054)：

假设`_config.yml`中的配置如下：

```yaml
deploy: 
 type: git
 repo: 
   Github1: 
     url: https://github.com/XXX/xxx.git
     branch: master
     message: "Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }}"
     name: XXX
     email: XXX@163.com
   Github2:
     url: https://github.com/YYY/yyy.git
     branch: master
     message: Site updated:{{ now('YYYY-MM-DD HH:mm:ss') }}
     name: YYY
     email: YYY@163.com
```



#### Travis-CI 

```yaml
... ...
script:
  - mkdir -p ~/.ssh/
  - echo "${HEXO_DEPLOY_PRIVATE_KEY}" > ~/.ssh/id_rsa 
  - chmod 600 ~/.ssh/id_rsa
  - ssh-keyscan github.com >> ~/.ssh/known_hosts
  - ssh-keyscan gitee.com >> ~/.ssh/known_hosts
  - ssh-keyscan coding.net >> ~/.ssh/known_hosts
  - hexo clean            
  - hexo generate         
  - hexo deploy
... ...
```

`HEXO_DEPLOY_PRIVATE_KEY`来自平台设置的变量。



#### Github Action

```yaml
... ...
jobs:
  Hexos-cicd:
    name: Hexos 构建和部署
    runs-on: ubuntu-latest 

    steps:
    ... ...
    - name: 设置部署私钥
      env:
        HEXO_DEPLOY_PRIVATE_KEY: ${{ secrets.HEXO_DEPLOY_PRIVATE_KEY }}
      run: |
        mkdir -p ~/.ssh/
        echo "$HEXO_DEPLOY_PRIVATE_KEY" > ~/.ssh/id_rsa 
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan github.com >> ~/.ssh/known_hosts
        ssh-keyscan gitee.com >> ~/.ssh/known_hosts
        ssh-keyscan coding.net >> ~/.ssh/known_hosts
    
    - name: 部署Hexo 
      run: |
        hexo clean
        hexo generate 
        hexo deploy
... ...
```

`secrets.HEXO_DEPLOY_PRIVATE_KEY`来自平台设置的变量。**需要用如上方式导出变量到环境中，才能以shell变量的形式读取它。**

将包含私钥的变量输出到文件中，然后赋予其权限。扫描平台的主机公钥到文件中，避免交互的提示。



### 令牌使用

[令牌使用参考示例](https://zhuanlan.zhihu.com/p/161969042)：

#### Travis-CI 

```yaml
# 环境变量
env:
 global:  
   - HEXOS_GITHUB_REF: github.com/XX/XX.github.io.git    
   - HEXOS_GITHUB_BRANCH: master
   - HEXOS_GITEE_REF: gitee.com/XX/XX.gitee.io.git
   - HEXOS_GITEE_BRANCH: master
   - HEXOS_CODING_REF: X.coding.net/XX/XX.coding.io.git
   - HEXOS_CODING_BRANCH: master

... ...

after_script: 
  #------------START: public处理 ------------#
  - cd ./public
  ... ...
  - git commit -m "Travis CI 自动构建在 `date +"%Y-%m-%d %H:%M"`"
  #-------------END: public处理 -------------#  
    
  #------------START: 推送至远端仓库 ------------#  
  # Github 
  - git push --force --quiet "https://${HEXOS_GITHUB_TOKEN}@${HEXOS_GITHUB_REF}" master:${HEXOS_GITHUB_BRANCH}
  
  # Gitee
  - git push --force --quiet "https://${HEXOS_GITEE_USER}:${HEXOS_GITEE_TOKEN}@${HEXOS_GITEE_REF}" master:${HEXOS_GITEE_BRANCH}
  
  # Coding
  - git push --force --quiet "https://${HEXOS_CODING_PROJECT_USER}:${HEXOS_CODING_PROJECT_TOKEN}@${HEXOS_CODING_REF}" master:${HEXOS_CODING_BRANCH}  
  #-------------END: 推送至远端仓库 -------------#
```

一些变量设置在平台中。

* `*_USER`
* `*_TOKEN`



#### Github Action

```yaml
# 环境变量
env:
  HEXOS_GITHUB_REF: github.com/XX/XX.github.io.git    
  HEXOS_GITHUB_BRANCH: master
  HEXOS_GITEE_REF: gitee.com/XX/XX.gitee.io.git
  HEXOS_GITEE_BRANCH: master
  HEXOS_CODING_REF: X.coding.net/XX/XX.coding.io.git
  HEXOS_CODING_BRANCH: master

... ...

jobs:
  Hexos-cicd:
    name: Hexos 构建和部署
    runs-on: ubuntu-latest 

    steps:
    ... ...
    # 将编译后的文件推送到指定仓库
    - name: 部署Hexos   
      run: |
        cd ./public && git init 
        ... ...
        git commit -m "GitHub Actions 自动构建在 `date +"%Y-%m-%d %H:%M"`"
        git push --force --quiet "https://${{ secrets.HEXOS_GITHUB_TOKEN }}@${HEXOS_GITHUB_REF}" master:${HEXOS_GITHUB_BRANCH}
        git push --force --quiet "https://${{ secrets.HEXOS_GITEE_USER }}:${{ secrets.HEXOS_GITEE_TOKEN }}@${HEXOS_GITEE_REF}" master:${HEXOS_GITEE_BRANCH}
        git push --force --quiet "https://${{ secrets.HEXOS_CODING_PROJECT_USER }}:${{ secrets.HEXOS_CODING_PROJECT_TOKEN }}@${HEXOS_CODING_REF}" master:${HEXOS_CODING_BRANCH} 
```

一些变量设置在平台中。

* `secrets.*_USER`
* `secrets.*_TOKEN`

注意： 读取平台中的变量的格式为`${{ secrets.XXX }}`





## 参考阅读


