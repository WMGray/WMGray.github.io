---
title: Hexo自动分类
categories:
  - 杂谈
tags:
  - 笔记
  - hexo
date: 2023-03-18 00:17:51
---
## 目的

给每一个笔记自动添加分类（习惯了使用文件夹分类）

## 自动分类

1. 使用的是[hexo-auto-category](https://link.zhihu.com/?target=https%3A//github.com/xu-song/hexo-auto-category)这个基于文件夹自动分类的插件，安装：

   ```
   npm install hexo-auto-category --save
   ```

2. 在`_config.yml`文件中添加配置：

   ```text
   # Generate categories from directory-tree
   # Dependencies: https://github.com/xu-song/hexo-auto-category
   # depth: the max_depth of directory-tree you want to generate, should > 0
   
   auto_category:
   	enable: true
   	depth:3
   ```

3. 除此之外最好修改一下 `_config.yml` 中的两处默认配置：

   ```yaml
   # 修改 permalink 让你的文章链接更加友好，并且有益于 SEO
   permalink: :year/:month/:hash.html
   
   # 规定你的新文章在 _post 目录下是以 cateory 
   new_post_name: :category/:title
   ```

4. 在每个文章的开头添加以下：

   > Obsidian可以创建`模板`

   ```
   ---
   title: {{title}}
   date: {{date}}
   categories: 
   tags:
   ---
   ```

5. 利用Git钩子函数触发更新

   - 查看当前仓库中 Git 钩子脚本的路径

     ```
     git config core.hooksPath
     ```

     > 如果是`~/.githooks`要改成`~/.git/hooks`

     ```
     git config core.hooksPath .git/hooks
     ```

   - 在`.git/hooks`目录下新建一个`pre-commit`文件

     1. 可以先在该文件中写入`echo hello world!`，然后在`git bash`执行`sh pre-commit`或者`./pre-commit`测试钩子能不能正常执行

     2. 没问题后，将如下命令写到文件里:

        ```yaml
        #!/bin/sh
        hexo generate && git add . 
        ```

        > 1. 之所以后面追加`git add .`，是因为generate后，所有文章的Front-matter信息会更新，所以要将所有修改重新添加进来
        > 2. 注意第一行一定要加上`#!/bin/sh`，这个不是注释！
        
        > 可是使用git commit 试试是否成功

   这样你新建一篇博客的工作流就简化为：

   1. 创建分类目录，写文章；
   2. 填写 `title`、`date`、`tag` 等元信息;
   3. 添加 git 工作区变更，并提交并推送代码到 github。



## 参考

- [Obsidian+Git完美维护Hexo博客](https://zhuanlan.zhihu.com/p/554333805)
- [Hexo + Obsidian + Git 完美的博客部署与编辑方案](https://segmentfault.com/a/1190000042111566)