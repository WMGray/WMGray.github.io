---
title: 安装docker
categories:
  - 杂谈
tags:
  - 笔记
  - docker
date: 2023-04-15 00:05:56
cover:
---
## 1. 安装docker

1. 卸载旧版本

   ```powershell
   sudo apt-get remove docker \
                  docker-engine \
                  docker.io
   ```

2. 使用 APT 安装

   由于 `apt` 源使用 HTTPS 以确保软件下载过程中不被篡改。因此，我们首先需要添加使用 HTTPS 传输的软件包以及 CA 证书。

   ```powershell
   sudo apt-get update
   
   sudo apt-get install \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
   ```

   鉴于国内网络问题，强烈建议使用国内源，官方源请在注释中查看。

   为了确认所下载软件包的合法性，需要添加软件源的 `GPG` 密钥。

   ```powershell
   curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   
   
   # 官方源
   # curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

   然后，我们需要向 `sources.list` 中添加 Docker 软件源

   ```powershell
   echo \
     "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   
   
   # 官方源
   # echo \
   #   "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
   #   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

   > 以上命令会添加稳定版本的 Docker APT 镜像源，如果需要测试版本的 Docker 请将 stable 改为 test。

3. 安装Docker

   ```powershell
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

4. 启动Docker

   ```powershell
   sudo systemctl enable docker
   sudo systemctl start docker
   docker -v	# 检查docker版本
   ```

5. 建立 docker 用户组

   默认情况下，`docker` 命令会使用 [Unix socket](https://en.wikipedia.org/wiki/Unix_domain_socket) 与 Docker 引擎通讯。而只有 `root` 用户和 `docker` 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 `root` 用户。因此，更好地做法是将需要使用 `docker` 的用户加入 `docker` 用户组。

   建立 `docker` 组：

   ```powershell
   sudo groupadd docker
   ```

   将当前用户加入 `docker` 组：

   ```powershell
   sudo usermod -aG docker $USER
   ```

   退出当前终端并重新登录，进行如下测试。

6. 测试docker是否安装成功

   ```powershell
   docker run --rm hello-world
   ```

   输出以下内容：

   ```
   Unable to find image 'hello-world:latest' locally
   latest: Pulling from library/hello-world
   b8dfde127a29: Pull complete
   Digest: sha256:308866a43596e83578c7dfa15e27a73011bdd402185a84c5cd7f32a88b501a24
   Status: Downloaded newer image for hello-world:latest
   
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   
   To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
       (amd64)
    3. The Docker daemon created a new container from that image which runs the
       executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
       to your terminal.
   
   To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash
   
   Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/
   
   For more examples and ideas, visit:
    https://docs.docker.com/get-started/
   ```



## 2. 迁移docker的默认安装（存储）路径

Docker版本（23.0.3）修改安装(存储)目录：通过修改(新建) /etc/docker/daemon.json ，指定 data-root 参数的值。

按如下操作：

```powershell
sudo vim /etc/docker/daemon.json
```

修改文件：

```
{
    "data-root": "/store/software/docker",
    "storage-driver": "overlay2"
}
```

> 将`data-root`改成你要迁移的路径

##  3. Docker -- GPU镜像

1. 安装docker--nvidia

   ```powershell
   # 1、添加源
   distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
   sudo curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
   sudo curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
   # 2、安装并重启
   sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
   sudo systemctl restart docker
   ```

2. 拉取docker镜像   

   >  从[nvidia/cuda]( https://hub.docker.com/r/nvidia/cuda)选择合适的镜像

   ```powershell
   docker pull nvidia/cuda:11.6.1-cudnn8-devel-ubuntu20.04
   ```

2. 创建容器

   ```
   docker run --name test -idt --gpus all nvidia/cuda:11.6.1-cudnn8-devel-ubuntu20.04
   ```

   也可以通过写"device=0"或者'"device=0,1,2,3"'来指定GPU

3. 进入容器

   ```powershell
   docker exec -it test  /bin/bash
   ```

​			使用`nvidia-smi`查看GPU信息

## 4. 安装Conda

1. 进入容器

   ```powershell
   docker exec -it test  /bin/bash
   ```

2. 进行基础的网络安装

   ```powershell
   apt-get update
   apt install net-tools        # ifconfig 
   apt install iputils-ping     # ping
   apt-get install -y wget		 # wget
   ```

3. 找anaconda安装包并下载  [conda官网](https://repo.anaconda.com/archive/)

   ```powershell
   wget https://repo.anaconda.com/archive/Anaconda3-5.3.0-Linux-x86_64.sh
   ```

4. 安装解压程序

   ```powershell
   apt-get install bzip2
   ```

5. 找到anaconda.sh并进行解压安装

   ```powershell
   chmod +x Anaconda3-5.3.0-Linux-x86_64.sh
   ./Anaconda3-5.3.0-Linux-x86_64.sh
   ```

   回车，一直yes ,vscode可以不进行安装

6. 进行环境变量配置

   ```powershell
   export PATH=$PATH:/root/anaconda3/bin	# 默认安装在root/anaconda3
   source ~/.bashrc
   ```

   使用`conda -V`查看版本信息

   ```powershell
   conda -V
   
   输出：conda 4.5.11
   ```

   `conda activate` 和 `conda deactivate` 报错：CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.

   输入以下命令：：

   ```powershell
   source activate
   source deactivate
   ```


##  5. 将本地环境部署到docker

在本地环境中将需要打包的代码复制到docker中：

```
# 获取docker中conda安装地址
conda info --env
# 输出：  /root/anaconda3
```

将本地环境复制到docker中（使用ctrl + d 退出）

```
docker cp your_env_path test://root/anaconda3/envs
```

再使用`docker cp`命令将代码复制到容器内

**保存所作的修改**！

```
docker commit -a 'author' -m 'instruction' test image_test
```

该命令各字段： test ：容器名字 image_test：保存的镜像的名字。

打包镜像

```
docker save -o test_tar.tar image_test
```

test_tar.tar: 压缩包名称 ， image_test： 镜像名称

加载已经打包好的镜像

```
docker load -i test_tar.tar
```

## 参考：

1. [Docker--从入门到实践](https://yeasy.gitbook.io/docker_practice/install/ubuntu)

1. [两种方法迁移 Docker 的默认安装(存储)目录](https://strikefreedom.top/archives/migrate-docker-installation-directory)
2. [Ubuntu18.04安装docker及nvidia docker](https://zhuanlan.zhihu.com/p/305952676)
3. 