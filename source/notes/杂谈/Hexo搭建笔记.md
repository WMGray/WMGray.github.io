---
title: Hexo搭建笔记
categories:
  - 杂谈
tags:
  - 笔记
  - hexo
date: 2023-03-18 00:18:02
---
## 搭建hexo 

1. 下载并安装node.js
   - 官网下载：https://nodejs.org/en/
   - 验证：node -v

2. 命令行安装cnpm
   - 命令：npm install -g cnpm --registry==https://registry.npm.taobao.org 
   - 验证：cnpm -v

3. 命令行安装hexo

   - 命令npm install -g hexo-cli

   - 验证：hexo -v


4. 建站

  ```
hexo init <folder>
cd <folder>
npm install
  ```

  新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

5. 配置[主题](https://hexo.io/themes/)

   > 主题的配置应放在`themes`文件夹下
   >
   > 上传前删除themes/.gitkeep

   > 在`_config.yml` 修改`theme`为你的主题名称



## Github + Hexo

1. 新建一个仓库，名为`用户名.github.io`

2. 本地hexo仓库关联远程GitHub仓库

3. 本地仓库一些必要的修改配置

   > 可以自动化给文章分类 `pre-commit`文件

   - 安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)

   ```
   npm install hexo-deployer-git --save
   ```

   - 修改`_config.yml`配置

     > url: https://all-smile.github.io/blog 
     >
     > root: /blog/ 
     >
     > ...
     >
     > deploy:  
     >
     > ​	type: 'git'  
     >
     > ​	repo: git@github.com:all-smile/blog.git  #这个是你的仓库的ssh
     >
     > ​	branch: gh-pages	# 要部署的分支，本教程博客源码在master分支，部署在gh-pages分支

   - 提交到远程仓库

4. 创建 `gh-pages` 分支

   hexo结合GitHub创建个人网站指定的分支名，hexo 内默认设置的分支也是叫这个名字

   ```bash
   git checkout -b gh-pages
   git push -u origin gh-pages
   ```

5. 远程仓库开启 github pages

   指定部署分支：gh-pages

   > 右上Setting --> Pages --> Branch -->选择gh-pages -->save

## 手动部署

- 命令：

  ```
  hexo clean
  hexo g
  hexo deploy
  ```

  hexo模板引擎生成静态文件，并推送到`gh-pages`分支下（替换原先分支下的所有文件）

  > 需要注意的是：`hexo deploy` 命令并不会帮助我们同步本地的修改到远程仓库，所以当在本地写完博文之后，要做两件事：一是发布站点，二是同步远程仓库，这样做比较麻烦，下面会讲解如何配置`持续集成`

## 自动部署--Github Actions

### 1. 设置 SSH 私钥 `deploy_key`

创建 SSH 部署密钥(使用git bash)

```
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
```

您将获得 2 个文件：

- gh-pages.pub是公钥

  - Settings --> Deploy Keys --> Add deploy key

    > name写为`public key of ACTIONS_DEPLOY_KEY`
    >
    > Key为.pub中的内容
    >
    > 勾选`Allow write acess`

- gh-pages是私钥

  - Settings --> Secrets and variavles --> Actions --> New repository secret

    > name为`ACTIONS_DEPLOY_KEY`
    >
    > value为gh-pages中的内容

- 不要上传私钥！！！

### 2. 新建 .github/workflows/pages.yml 文件

文件内容如下：

```
name: Pages

# 触发器、分支
on:
  push:
    branches:
      - master  # default branch
jobs:
  # 子任务
  pages:
    runs-on: ubuntu-latest # 定运行所需要的虚拟机环境
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v2
        # with:
        #   submodules: true
        #   fetch-depth: 0
      # 每个name表示一个步骤:step
      - name: Use Node.js 16.x
        uses: actions/setup-node@v2
        with:
          node-version: '16.14.1' # 自己正在使用的node版本即可
      # - run: node -v # 查看node版本号
      # 缓存依赖项: https://docs.github.com/cn/actions/using-workflows/caching-dependencies-to-speed-up-workflows
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          # npm cache files are stored in `~/.npm` on Linux/macOS
          path: ~/.npm
          # path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      # 查看路径 : /home/runner/work/blog/blog
      # - name: Look Path
      #   run: pwd
      # 查看文件
      - name: Look Dir List
        run: tree -L 3 -a
      # 第一次或者依赖发生变化的时候执行 Install Dependencies，其它构建的时候不需要这一步
      - name: Install Dependencies
        run: npm install
      - name: Look Dir List
        run: tree -L 3 -a
      # - name: clean theme cache
      #   run: git rm -f --cached themes/tenacity
        # run: git submodule deinit themes/tenacity && git rm themes/tenacity
      # 安装主题
      - name: Install Theme
        run: git submodule add https://github.com/all-smile/tenacity.git themes/tenacity
      - name: Clean
        run: npm run clean
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY  }}
          user_name: xxxx	# 需要修改
          user_email: xx@xx.com	#需要修改
          # 获取提交文章源码时的commit message，作为发布gh-pages分支的信息
          commit_message: ${{ github.event.head_commit.message }}
          full_commit_message: ${{ github.event.head_commit.message }}
          github_token: ${{ secrets.GITHUB_TOKEN }}
          # GITHUB_TOKEN不是个人访问令牌，GitHub Actions 运行器会自动创建一个GITHUB_TOKEN密钥以在您的工作流程中进行身份验证。因此，您无需任何配置即可立即开始部​​署
          publish_dir: ./public
          allow_empty_commit: true # 允许空提交
      # Use the output from the `deploy` step(use for test action)
      - name: Get the output
        run: |
          echo "${{ steps.deploy.outputs.notify }}"

```

### 3. 修改 `_config.yml` 文件中的`Deploy`配置

```
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: 'git'
  repo: .......
  branch: gh-pages
  # 默认提交信息： Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }}
  message: ${{ github.event.head_commit.message }} # 直接将提交消息传输到 GitHub Pages 存储库
```



上传blog时记得切换回`master`分支



## 参考

- [Hexo+GitHub搭建个人博客，实现云端编辑、一键发文](https://www.cnblogs.com/all-smile/p/16608503.html)
- [hexo](https://hexo.io/docs/)
- [2021最全hexo搭建博客+matery美化+使用（保姆级教程）](https://www.bilibili.com/read/cv12633102)

