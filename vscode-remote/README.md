# VSCode Remote

本项目为`VSCode Remote SSH`插件的配套开发环境，用于配合`VSCode`实现远程开发。

默认开发环境为`Ubuntu`最新的LTS版本，同时也提供用于深度学习的包含`CUDA`和`CUDNN`的版本。

提示：运行GPU版本需要`nvidia-docker`支持，参见[官方说明](https://github.com/NVIDIA/nvidia-docker)。不同`GPU`对于`CUDA`的版本支持情况不同，参见[这里](https://github.com/NVIDIA/nvidia-docker/wiki/Installation-(Native-GPU-Support)#usage)。

## CPU说明

```sh
# 编译镜像
$ cd build
$ docker build -t xczh/vscode-remote .

# 运行容器
# Volume: coder-vscr，可通过 docker volumes ls 查看
$ docker run -d --restart=unless-stopped \
                --name vscode-remote \
                --hostname vscode-remote \
                --cap-add SYS_PTRACE \
                -v coder-vscr:/home/coder \
                -p 2201:22 \
                -e SSH_CODER_PASSWORD=YOUR_LONG_PASSWORD \
                xczh/vscode-remote:latest
```

可用的环境变量：
 - SSH_CODER_PASSWORD: 用户`coder`的SSH登录密码（eg. Hah9eigh）
 - SSH_CODER_PUBLICKEY: 用户`coder`的SSH登录公钥（eg. ssh-rsa XXXX）

## GPU版本说明

```sh
# 编译GPU版本镜像
$ BASE_IMAGE="nvidia/cuda:10.1-cudnn7-devel" \
  TAG="nvidia-cuda10.1-cudnn7" \
  docker build --build-arg BASE_IMAGE=${BASE_IMAGE} -t xczh/vscode-remote:${TAG} .

# 运行容器
$ docker run -d --restart=unless-stopped \
                --name vscode-remote \
                --hostname vscode-remote \
                --cap-add SYS_PTRACE \
                --gpus all \
                -v coder-vscr:/home/coder \
                -p 2201:22 \
                -e SSH_CODER_PASSWORD=YOUR_LONG_PASSWORD \
                xczh/vscode-remote:nvidia-cuda10.1-cudnn7
```

## 本地环境设置

1. 安装`Visual Studio Code`，并安装好`Remote - SSH`扩展

2. 在**系统命令行**中测试连接:

```
# 首次，若未设置SSH_CODER_PUBLICKEY，需使用密码登陆
$ ssh coder@vscode-remote

# 下载私钥文件保存到本地
coder@vscode-remote:~$ cat ~/.ssh/id_ed25519

# 下一次登录，使用私钥登陆。若一切顺利，此时不会再询问密码。
$ ssh -i /path/to/id_ed25519 coder@vscode-remote
```

3. 测试成功可以正常免密码登陆后，在`Visual Studio Code`中使用左下角的绿色`Remote - SSH`扩展连接`coder@vscode-remote`，完毕！