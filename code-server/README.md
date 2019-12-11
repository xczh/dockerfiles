# Code-Server

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/xczh/code-server)](https://hub.docker.com/r/xczh/code-server/tags)
[![](https://images.microbadger.com/badges/image/xczh/code-server.svg)](https://microbadger.com/images/xczh/code-server)

[code-server](https://github.com/cdr/code-server)将Microsoft VSCode运行在远程服务器上，通过浏览器访问，实现了WebIDE的基本功能。

相比于[官方镜像](https://hub.docker.com/r/codercom/code-server)，本镜像主要有以下区别：
 - 设置时区为Asia/Shanghai
 - 设置`APT`源为`mirrors.tuna.tsinghua.edu.cn`
 - 内置了常用系统工具（具体见Dockerfile），方便了WebIDE开发
 - 默认监听容器内`0.0.0.0:8080`
 - `init`使用`docker`官方的`tini`替代官方镜像的`dumb-init`

编译说明：本项目已在DockerHub上自动编译，建议直接拉取预编译镜像运行而非手动编译，可用的Tags在[这里](https://hub.docker.com/r/xczh/code-server/tags)。

## 使用说明

### 拉取预编译镜像

```
# 推荐，内置C/C++, Python3, PHP, Golang, NodeJS开发环境
$ sudo docker pull xczh/code-server:latest

# 基础系统环境，无内置编程语言，可自行安装配置
$ sudo docker pull xczh/code-server:minimal

# Nvidia GPU深度学习环境，使用前请参考: https://github.com/NVIDIA/nvidia-docker
$ sudo docker pull xczh/code-server:nvidia-cuda10.2-cudnn7
```

### 运行

可用的环境变量：
 - `PORT` Web服务监听端口，默认`8080`
 - `PASSWORD` Web服务访问密码，默认`none`

```sh
# CPU版本
$ sudo docker run -d --restart=unless-stopped \
                  --name code-server \
                  --hostname code-server \
                  --cap-add SYS_PTRACE \
                  -e PASSWORD=YOUR_VERY_LONG_PASSWORD \
                  -p 8080:8080 \
                  -v code-server:/home/coder \ 
                  xczh/code-server:latest

# Nvidia GPU深度学习版本
# 请参考: https://github.com/NVIDIA/nvidia-docker/wiki/Installation-(Native-GPU-Support)#usage
# 执行 nvidia-smi 命令检查GPU环境是否正常
$ sudo docker run -d --restart=unless-stopped \
                  --name code-server \
                  --hostname code-server \
                  --cap-add SYS_PTRACE \
                  --gpus all \
                  -e PASSWORD=YOUR_VERY_LONG_PASSWORD \
                  -p 8080:8080 \
                  -v code-server:/home/coder \
                  xczh/code-server:nvidia-cuda10.2-cudnn7
```