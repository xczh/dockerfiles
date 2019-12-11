# dnsmasq

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/xczh/dnsmasq)](https://hub.docker.com/r/xczh/dnsmasq/tags)
[![](https://images.microbadger.com/badges/image/xczh/dnsmasq.svg)](https://microbadger.com/images/xczh/dnsmasq)

`dnsmasq`是一个轻量级递归DNS解析器，常用于DNS本地缓存和中小型局域网DNS本地解析。

本项目包含[dnsmasq](http://www.thekelleys.org.uk/dnsmasq/)和[dnsproxy](https://github.com/AdguardTeam/dnsproxy)：
 - `dnsmasq` 负责DNS本地缓存和本地DNS域名解析，将其他的域名解析请求转发给`dnsproxy`
 - `dnsproxy` 负责通过`DoT`和`DoH`等安全协议向上游DNS发起查询

配合使用[dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)项目，可便捷实现国内外DNS分流解析，以充分利用CDN加速功能访问国内站点。

编译说明：本项目已在DockerHub上自动编译，建议直接拉取预编译镜像运行而非手动编译，可用的Tags在[这里](https://hub.docker.com/r/xczh/dnsmasq/tags)。

## 使用说明

```sh
# 编译镜像
$ cd build
$ docker build --force-rm -t xczh/dnsmasq .

# 运行容器
$ docker run -d --restart=unless-stopped \
             --name dnsmasq \
             --hostname dnsmasq \
             --log-opt "max-size=100m" \
             -p 80:80 \
             -p 53:53/udp \
             -v $(pwd)/data/dnsmasq.conf:/etc/dnsmasq.conf \
             -v $(pwd)/data/dnsmasq.d:/etc/dnsmasq.d \
             -e WEBPROC_USER=admin \
             -e WEBPROC_PASS=admin \
             -e WEBPROC_CONF=/etc/dnsmasq.conf \
             xczh/dnsmasq
```

可用的环境变量参考`build/run-server`。
