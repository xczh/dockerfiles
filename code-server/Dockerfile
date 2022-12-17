ARG BASE_IMAGE=ubuntu:22.04
ARG CODE_RELEASE

FROM ${BASE_IMAGE}

LABEL maintainer.name="xczh" \
      maintainer.email="xczh.me@foxmail.com" \
      description="Code-Server WebIDE"

ENV LANG=en_US.UTF-8 CODE_AUTH=password PASSWORD=password CODE_ARGS=

RUN apt-get update && \
    LANG=C.UTF-8 DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        apt-transport-https \
        apt-utils \
        ca-certificates \
        curl \
        file \
        git \
        git-lfs \
        gnupg \
        htop \
        inetutils-ping \
        locales \
        man-db \
        nano \
        net-tools \
        openssl \
        sudo \
        unzip \
        vim \
        wget \
        xauth \
        zip \
        && \
    locale-gen en_US.UTF-8 zh_CN.UTF-8 && \
    update-locale LANG=en_US.UTF-8 && \
    if [ -z ${CODE_RELEASE+x} ]; then \
      CODE_RELEASE=$(curl -sX GET https://api.github.com/repos/coder/code-server/releases/latest \
        | awk '/tag_name/{print $4;exit}' FS='[""]' | sed 's|^v||'); \
    fi && \
    curl -o /tmp/code-server.tar.gz -L \
      "https://github.com/coder/code-server/releases/download/v${CODE_RELEASE}/code-server-${CODE_RELEASE}-linux-amd64.tar.gz" && \
    mkdir -p /app /volume/workspace && \
    tar -xvf /tmp/code-server.tar.gz -C /app --strip-components=1 && \
    apt-get clean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cache && \
    adduser --gecos '' --disabled-password coder && \
    echo "coder ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/nopasswd && \
    chown -R coder:coder /volume

USER coder

WORKDIR /volume/workspace

EXPOSE 8080

CMD /app/bin/code-server --disable-telemetry --bind-addr 0.0.0.0:8080 --auth ${CODE_AUTH} --user-data-dir /volume/data --extensions-dir /volume/extensions ${CODE_ARGS} /volume/workspace
