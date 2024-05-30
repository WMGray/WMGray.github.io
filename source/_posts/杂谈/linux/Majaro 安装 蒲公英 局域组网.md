---
title: Majaro 安装 蒲公英 局域组网
date: 2024-05-30
categories:
  - linux
  - 局域网
tags:
  - 笔记
cover:
---
[贝锐蒲公英软件客户端最新版官方下载，蒲公英联机组网平台软件下载 - 贝锐蒲公英官网](https://pgy.oray.com/download)

- Docker 方式安装 -- 按照官方安装方式即可
```
docker pull bestoray/pgyvpn  # 拉取镜像

# systemctl enable docker.service # 设置docker自启动
# PGY_USERNAME 为 贝锐账号 ”或”UID“
docker run -d --restart=always --device=/dev/net/tun --net=host --cap-add=NET_ADMIN --env PGY_USERNAME="xxx" --env PGY_PASSWORD="xxx" bestoray/pgyvpn  # always -- docker容器自启动
```
- 使用安装包安装
> 由于蒲公英官网并未提供 Arch 的安装包，故采用下载 .deb 安装包，然后转换的形式
1. 下载安装包
	```
	wget https://pgy.oray.com/softwares/153/download/2156/PgyVisitor_6.2.0_arm64.deb
	```
2. 安装 **`debtap`** 包
	```
	sudo pacman -S debtap
	yay -S debtap # 或者
	```
3. 更新 `debtap` 源
	```
	sudo debtap -u
	```
4. 将 `.deb` 包转换成 `.pkg.tar.zst`
	```
	sudo debtap xxx.deb
	```
5. 安装蒲公英
	```
	sudo pacman -S xxx.pkg.tar.zst
	```
	不用管报错信息
	```
	pgyvisitor # 运行命令
	```
登录用户，设置自动启动， over


