ARG BASE_IMAGE=ubuntu:latest

# Stage 1: Use the Pre-build Binary
FROM codercom/code-server:latest AS cdr

# Stage 2: Final
FROM ${BASE_IMAGE}

LABEL maintainer.name="xczh" \
      maintainer.email="xczh.me@foxmail.com" \
      description="Code-server WebIDE"

ENV LANG=zh_CN.UTF-8

COPY --from=cdr /usr/local/bin/code-server /usr/local/bin/code-server

RUN rm -rf /var/lib/apt/lists/* && \
    apt-get update && \
    LANG=C.UTF-8 DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        apt-transport-https \
        apt-utils \
        ca-certificates \
        curl \
        file \
        git \
        gnupg \
        htop \
        inetutils-ping \
        locales \
        man-db \
        nano \
        net-tools \
        openssl \
        sudo \
        tzdata \
        unzip \
        vim \
        wget \
        xauth \
        zip \
        && \
    echo "Asia/Shanghai" > /etc/timezone && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime.bak && \
    locale-gen en_US.UTF-8 zh_CN.UTF-8 && \
    update-locale LANG=zh_CN.UTF-8 && \
    sed -i 's/http:\/\/archive.ubuntu.com/https:\/\/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list && \
    sed -i 's/http:\/\/security.ubuntu.com/https:\/\/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list && \
    curl -sSL -o /sbin/tini https://github.com/krallin/tini/releases/download/v0.18.0/tini && \
    chmod a+x /sbin/tini && \
    DEBIAN_FRONTEND=noninteractive apt-get remove --purge -y tzdata && \
    apt-get clean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cache && \
    mv /etc/localtime.bak /etc/localtime && \
    adduser --gecos '' --disabled-password coder && \
    echo "coder ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/nopasswd

COPY root /

USER coder

WORKDIR /home/coder

VOLUME ["/home/coder"]

ENV PORT=8080

EXPOSE 8080

ENTRYPOINT ["/sbin/tini", "--", "/usr/sbin/entrypoint"]

CMD "code-server --host 0.0.0.0 --port ${PORT} --auth password --user-data-dir ~/.code-user-data --disable-telemetry --locale zh-CN"