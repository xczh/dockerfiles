# Code-Server

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/xczh/code-server)](https://hub.docker.com/r/xczh/code-server/tags)
[![](https://images.microbadger.com/badges/image/xczh/code-server.svg)](https://microbadger.com/images/xczh/code-server)

[code-server](https://github.com/coder/code-server)将Microsoft VS Code运行在远程服务器上，通过浏览器访问，实现了WebIDE的基本功能。

## 使用说明

### 编译镜像

```
# amd64架构
$ sudo docker build -t xczh/code:latest .

# 非amd64架构的需使用对应体系的Dockerfile
$ sudo docker build -t xczh/code:latest -f Dockerfile.arm64 .
```

### 运行

可用的环境变量：
 - `CODE_AUTH` 可选值`password`,`none`，默认`password`
 - `PASSWORD` 若使用`CODE_AUTH`为`password`，则该变量为HTTP Basic Auth的明文密码，默认为`password`
 - `HASHED_PASSWORD` 若使用`CODE_AUTH`为`password`，则该变量为HTTP Basic Auth的密码哈希值，使用[Argon2](https://argon2.online/)算法生成，默认为空
 - `CODE_ARGS` 其他启动参数，默认为空

```sh
$ sudo docker run -d --restart=unless-stopped \
                  --name code \
                  --hostname code-server \
                  --init \
                  --cap-add SYS_PTRACE \
                  -e PASSWORD=password \
                  -p 8080:8080 \
                  -v code:/volume \
                  xczh/code:latest
```
