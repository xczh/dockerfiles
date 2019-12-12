# Stage 1: Build vlmcsd from source
FROM alpine:edge as builder

WORKDIR /root

RUN apk add --no-cache git make build-base && \
    git clone --depth 1 --single-branch https://github.com/Wind4/vlmcsd.git && \
    cd vlmcsd/ && \
    make

# Stage 2: Final
FROM alpine:edge

LABEL maintainer.name="xczh" \
      maintainer.email="xczh.me@foxmail.com" \
      description="KMS Emulator in C"

COPY --from=builder /root/vlmcsd/bin/vlmcsd /root/vlmcsd/bin/vlmcs /usr/local/bin/

ENV ARGS=

EXPOSE 1688

CMD vlmcsd -D -d -e -v -u nobody -g nobody ${ARGS}