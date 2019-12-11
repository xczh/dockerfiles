ARG BASE_IMAGE=ubuntu:latest

FROM ${BASE_IMAGE}

LABEL maintainer.name="xczh" \
      maintainer.email="xczh.me@foxmail.com" \
      description="VSCode-Remote Develop Envionment"

ENV LANG=en_US.UTF-8

RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends apt-utils && \
    LANG=C.UTF-8 DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        apt-transport-https \
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
        openssh-server \
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
    update-locale LANG=en_US.UTF-8 && \
    sed -i 's/http:\/\/archive.ubuntu.com/https:\/\/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list && \
    sed -i 's/http:\/\/security.ubuntu.com/https:\/\/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list && \
    mkdir /run/sshd && \
    curl -sSL -o /sbin/tini https://github.com/krallin/tini/releases/download/v0.18.0/tini && \
    chmod a+x /sbin/tini && \
    DEBIAN_FRONTEND=noninteractive apt-get remove --purge -y tzdata && \
    apt-get clean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cache && \
    rm -vf /etc/ssh/ssh_host_* && \
    mv /etc/localtime.bak /etc/localtime && \
    adduser --gecos '' --disabled-password coder && \
    echo "coder ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/nopasswd

COPY root /

VOLUME ["/home/coder"]

WORKDIR /home/coder

EXPOSE 22

ENTRYPOINT ["/sbin/tini", "--", "/usr/sbin/entrypoint"]

CMD ["/usr/sbin/sshd", "-D"]