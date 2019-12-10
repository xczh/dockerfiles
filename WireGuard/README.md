# WireGuard 

WireGuard是一个基于UDP的点对点VPN，Linux下有用户态和内核态两种工作模式。

内核态下，WireGuard以内核模块形式工作，需要编译并加载`wireguard.ko`内核模块，项目地址在这里[wireguard](https://git.zx2c4.com/WireGuard/about/)。

用户态下，WireGuard采用Go语言编写，项目地址在这里[wireguard-go](https://git.zx2c4.com/wireguard-go/about/)。

本项目的目的是，在不污染系统环境（不安装`gcc`工具链/`内核头文件`等）的情况下，编译必要的程序，并复制到系统对应目录。

## 编译内核模块

在容器内完成编译，并将编译好的内核模块`wireguard.ko`及相关的工具（`wg`和`wg-quick`）安装到系统对应位置。

对于内核模块而言，编译时和运行时的内核版本必须完全一致，因此请根据Linux发行版使用对应的基础镜像。

编译后生成的文件：
 - 内核模块`wireguard.ko`，位于`/lib/modules`
 - 用户工具`wg`和`wg-quick`，位于`/usr/bin`
 - systemd服务文件`wg-quick@.service`，位于`/usr/lib/systemd`
 - man帮助文件，位于`/usr/share/man`
 - Bash命令自动补全脚本，位于`/usr/share/bash-completion`

通常，除内核模块以外，其他工具都位于`wireguard-tools`包内。该包可单独在系统中安装。

以`Ubuntu`为例，挂载系统目录，并进行编译：

```s
$ docker run -it --rm \
             --name wg \
             --cap-add sys_module \
             --cap-add net_admin \
             -v /lib/modules:/lib/modules \
             ubuntu:latest \
             bash

# 此处进入容器环境

# 对于Ubuntu Eoan (19.10) 及以上，直接安装
root@wg $ apt-get update
root@wg $ apt-get install -y wireguard

# 其他的发行版，通过PPA安装
root@wg $ add-apt-repository ppa:wireguard/wireguard
root@wg $ apt-get update
root@wg $ apt-get install -y wireguard

# 测试加载内核模块
root@wg $ modprobe wireguard
root@wg $ lsmod | grep wireguard
# 看到有wireguard字样即为加载成功

# 测试用户工具wg及wg-quick
```

其他发行版：

Fedora:

```s
$ sudo dnf copr enable jdoss/wireguard
$ sudo dnf install wireguard-dkms wireguard-tools
```

Debian:

```s
$ echo "deb http://deb.debian.org/debian/ unstable main" > /etc/apt/sources.list.d/unstable.list
$ printf 'Package: *\nPin: release a=unstable\nPin-Priority: 90\n' > /etc/apt/preferences.d/limit-unstable
$ apt update
$ apt install wireguard
```