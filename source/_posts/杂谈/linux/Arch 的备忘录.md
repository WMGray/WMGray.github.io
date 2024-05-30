---
title: Arch 的备忘录
date: 2024-05-30
categories:
  - linux
  - 杂谈
tags:
  - 笔记
  - linux
cover:
---
#### Fish Shell

1. 安装 `fish`
	```
	sudo pacman -S fish
	```
2. 设置 Fish Shell 为默认 shell
	```
	chsh -s /usr/bin/fish
	```

#### 安装 miniconda 3
[Miniconda — Anaconda documentation](https://docs.anaconda.com/free/miniconda/index.html)

1. 安装 
{% tag color:warning 从官网下载的 [Miniconda3 Linux-aarch64 64-bit](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh) 会报错 %}
参考: [Illegal Instruction error when verifying Anaconda/Miniconda Install - Stack Overflow](https://stackoverflow.com/questions/68213186/illegal-instruction-error-when-verifying-anaconda-miniconda-install)
选择下载 [py39_4.92](https://repo.anaconda.com/miniconda/Miniconda3-py39_4.9.2-Linux-aarch64.sh)
```
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.9.2-Linux-aarch64.sh  # 下载
sudo bash ./Miniconda3-py39_4.9.2-Linux-aarch64.sh
```
一路 enter 下去即可，这里选择的安装地址是 `/opt/miniconda3`
1. 配置
{% mark color:blue Bash %}

```
# 在/home/xxx/.bashrc 文件中添加
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/opt/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/opt/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/opt/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="/opt/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<

# 保存文件后运行
source .bashrc
```

{% mark color:blue Fish %}

```
# Bash shell
conda init fish
# 配置文件在/home/xxx/.config/fish/config.fish
set -gx CONDA_LEFT_PROMPT 1  # 在左侧显示环境名称
```