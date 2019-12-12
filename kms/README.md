# Windows Key Management Service (KMS)

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/xczh/kms)](https://hub.docker.com/r/xczh/kms/tags)
[![](https://images.microbadger.com/badges/image/xczh/kms.svg)](https://microbadger.com/images/xczh/kms)

本项目用于`Windows`和`Office`系列`VOL`产品的激活，使用`vlmcs`作为激活服务器。

`vlmcs`项目地址: https://forums.mydigitallife.net/threads/50234

> vlmcs
>
> KMS Emulator in C (currently runs on Linux including Android, FreeBSD, > Solaris, Minix, Mac OS, iOS, Windows with or without Cygwin) 

## 使用说明

```sh
$ docker run -d --restart=unless-stopped \
                --name kms \
                --hostname kms \
                -v /etc/timezone:/etc/timezone:ro \
                -v /etc/localtime:/etc/localtime:ro \
                -p 1688:1688 \
                xczh/kms
```