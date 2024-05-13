---
title: nonebot搭建QQ机器人
categories:
  - 杂谈
tags:
  - NoneBot
  - 笔记
  - QQ机器人
cover: workout,strava
date: 2023-03-20 18:20:26
---

## 框架介绍

NoneBot2 是一个现代、跨平台、可扩展的 Python 聊天机器人框架，它基于 Python 的类型注解和异步特性，能够为你的需求实现提供便捷灵活的支持。

[nonebot官方文档](https://v2.nonebot.dev/docs/)

## 搭建步骤

> 请确保你的 Python 版本 >= 3.8

### 1. 通过脚手架安装nb

1. 安装 [pipx](https://pypa.github.io/pipx/)

   ```powershell
   python -m pip install --user pipx
   python -m pipx ensurepath
   ```

2. 安装脚手架

   ```powershell
   pipx install nb-cli
   ```

   > 如果提醒以下内容：
   >
   > ```
   >  installed package nb-cli 1.0.5, installed using Python 3.10.9
   >   These apps are now globally available
   >     - nb.exe
   > ⚠️  Note: 'C:\\Users\\WMGray\\.local\\bin' is not on your PATH environment variable. These apps will not be globally accessible until
   >     your PATH is updated. Run `pipx ensurepath` to automatically add it, or manually modify your PATH in your shell's config file
   >     (i.e. ~/.bashrc).
   > done! ✨ 🌟 ✨
   > ```
   >
   > 请按照上面的警告输入命令：Run `pipx ensurepath` to automatically add it, or manually modify your PATH in your shell's config file
   >
   > 然后根据接下来的输出进行

### 2.创建nb项目

   输入`nb`来进行交互

   - 选择`创建一个NoneBot`项目
   - 选择模板
   - 输入项目名称
   - 选择适配器--FastAPI
   - 选择驱动器--OneBotV11

### 3.项目配置

   在.env更改配置项

   ```yaml
   HOST=127.0.0.1  # 配置 NoneBot2 监听的 IP/主机名
   PORT=8080  # 配置 NoneBot2 监听的端口
   SUPERUSERS=["123456789", "987654321"]  # 配置 NoneBot 超级用户
   NICKNAME=["awesome", "bot"]  # 配置机器人的昵称
   COMMAND_START=["/", ""]  # 配置命令起始字符
   COMMAND_SEP=["."]  # 配置命令分割字符
   ```

   其他参数见[NoneBot配置](https://v2.nonebot.dev/docs/tutorial/configuration)

### 4.下载gocq

   [go-cqhttp](https://github.com/Mrs4s/go-cqhttp/releases)选择合适的版本

   ![image-20230320191217942](https://raw.githubusercontent.com/WMGray/blog.images/master/images/image-20230320191217942.png)

   - 双击exe生成运行脚本

   - 选择通信方式（本文选择的反向WebSocket）

   - 若要支持任意格式语音发送，可安装ffmpeg

     - 从 [这里](https://www.gyan.dev/ffmpeg/builds/ffmpeg-release-full.7z) 下载 并解压, 并为 `bin` 这个文件夹添加环境变量

     - 然后在 cmd 输入 **(不能使用 powershell）**

       ```
       setx /M PATH "C:\Program Files\ffmpeg\bin;%PATH%"
       # 自行将这个指令中的 C:\Program Files 替换成你的解压目录
       ```

   - 生成config.yaml后修改以下信息

     ```yaml
     uin: 1233456 # QQ账号
     password: '' # 密码为空时使用扫码登录
     ....  
     universal: ws://127.0.0.1:端口号/onebot/v11/ws/	#端口号同bot项目下的.env端口
     ```

### 5. 启动bot

    ```powershell
    nb run	# 在项目界面
    ```

同时，运行`go-cqhttp.bat` 
