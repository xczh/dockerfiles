# Gitea

`Gitea`用于提供轻量级自建`git`服务。

由于[官方镜像](https://hub.docker.com/r/gitea/gitea/tags)编译较为复杂，为了保证上游兼容性，建议直接使用`gitea/gitea`镜像启动容器。

## 使用说明

```sh
$ docker run -d --restart=unless-stopped \
                --name gitea \
                --hostname gitea \
                -p 3000:3000 \
                -p 3022:22 \
                -v gitea:/data \
                gitea/gitea
```

## 特别说明

官方镜像默认使用了两个端口`SSH: 22`和`HTTP: 3000`，容器内的运行用户为`git`（`UID=1000,GID=1000`），特别需要注意：

1. 若修改HTTP监听端口为`1024`以下的特权端口，需要在`docker run`命令加入以下参数以允许非`root`用户使用特权端口：

```sh
$ docker run --sysctl net.ipv4.ip_unprivileged_port_start=0 ...
```

2. VOLUME目录为`/data`，如果将已有的数据和配置文件挂载到该目录，需要确保文件权限正确，否则会启动失败：

```sh
$ cd /path/to/data
# 使用UID和GID修改文件所有者
$ chown -R 1000.1000 git/ gitea/
$ chown -R 0.0 ssh/ 
```
