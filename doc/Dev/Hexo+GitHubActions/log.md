[Hexo+GitHub Actions 完美打造个人博客](https://www.cnblogs.com/i-code/p/12869046.html)





[GitHub Action + Hexo实现在线写作](https://blog.csdn.net/qq_41426117/article/details/108703295)

```yaml
# workflow name
name: Hexo Blog CI

# master branch on push, auto run
on: 
  push:
    branches:
      - master
      
jobs:
  build: 
    runs-on: ubuntu-latest 
        
    steps:
    # check it to your workflow can access it
    # from: https://github.com/actions/checkout
    - name: Checkout Repository master branch
      uses: actions/checkout@master 
      
    # from: https://github.com/actions/setup-node  
    - name: Setup Node.js 12.x 
      uses: actions/setup-node@master
      with:
        node-version: "12.x"
    
    - name: Setup Hexo Dependencies
      run: |
        npm install hexo-cli -g
        npm install
    
    - name: Setup Deploy Private Key
      env:
        HEXO_DEPLOY_PRIVATE_KEY: ${{ secrets.HEXO_DEPLOY_PRIVATE_KEY }}
      run: |
        mkdir -p ~/.ssh/
        echo "$HEXO_DEPLOY_PRIVATE_KEY" > ~/.ssh/id_rsa 
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan github.com >> ~/.ssh/known_hosts
        
    - name: Setup Git Infomation
      run: | 
        git config --global user.name '<你的用户名>' 
        git config --global user.email '<你登录的email>'
    - name: Deploy Hexo 
      run: |
        hexo clean
        hexo generate 
        hexo deploy
```



[利用GitHub+Actions自动部署Hexo博客](https://blog.csdn.net/u012208219/article/details/106883054)

```yaml
# workflow name
name: Hexo Blog CI

# master branch on push, auto run
on: 
  push:
    branches:
      - master
      
jobs:
  build: 
    runs-on: ubuntu-latest 
        
    steps:
    # check it to your workflow can access it
    # from: https://github.com/actions/checkout
    - name: Checkout Repository master branch
      uses: actions/checkout@master 
      
    # from: https://github.com/actions/setup-node  
    - name: Setup Node.js 10.x 
      uses: actions/setup-node@master
      with:
        node-version: "10.x"
    
    - name: Setup Hexo Dependencies
      run: |
        npm install hexo-cli -g
        npm install
    
    - name: Setup Deploy Private Key
      env:
        HEXO_DEPLOY_PRIVATE_KEY: ${{ secrets.HEXO_DEPLOY_PRIVATE_KEY }}
      run: |
        mkdir -p ~/.ssh/
        echo "$HEXO_DEPLOY_PRIVATE_KEY" > ~/.ssh/id_rsa 
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan github.com >> ~/.ssh/known_hosts
        
    - name: Setup Git Infomation
      run: | 
        git config --global user.name '名字' 
        git config --global user.email '邮件'
    - name: Deploy Hexo 
      run: |
        hexo clean
        hexo generate 
        hexo deploy
```



[Hexo 使用 Github Actions 自动更新](https://www.jianshu.com/p/7940fe40885d)

需要两个 github 仓库：

* 一个用于发布页面： XXXXXX.github.io
* 一个用于放源码： hexo-source （可设为隐私仓库）

```yaml
name: Hexo Auto-Deploy
on: [push]

jobs:
  build:
    name: Hexo Auto-Deploy by GitHub Actions
    runs-on: ubuntu-latest

    steps:
    - name: 1. git checkout...
      uses: actions/checkout@v1
      
    - name: 2. setup nodejs...
      uses: actions/setup-node@v1
    
    - name: 3. install hexo...
      run: |
        npm install hexo-cli -g
        npm install
        
    - name: 4. hexo generate public files...
      run: |
        hexo clean
        hexo g  

    - name: 5. hexo deploy ...
      run: |
        mkdir -p ~/.ssh/
        echo "${{ secrets.GITHUB_ACTION }}" > ~/.ssh/id_rsa
        chmod 600 ~/.ssh/id_rsa
        ssh-keyscan github.com >> ~/.ssh/known_hosts
        
        git config --global user.name "XXXXXX"
        git config --global user.email "XXXXXX@XXXXXX.com"
        git config --global core.quotepath false
        
        hexo d
```



