### Stage 1: Build
FROM golang:1-alpine as builder

RUN apk add --no-cache git && \
    go get -d -v github.com/AdguardTeam/dnsproxy && \
    cd /go/src/github.com/AdguardTeam/dnsproxy && \
    DNSPROXY_LATEST_TAG=$(git describe --tags `git rev-list --tags --max-count=1`) && \
    git checkout ${DNSPROXY_LATEST_TAG} && \
    CGO_ENABLED=0 go build -mod=vendor -ldflags "-X main.VersionString=${DNSPROXY_LATEST_TAG}" -o /go/bin/dnsproxy && \
    cd /go && \
    go get -d -v github.com/jpillora/webproc && \
    cd /go/src/github.com/jpillora/webproc && \
    WEBPROC_LATEST_TAG=$(git describe --tags `git rev-list --tags --max-count=1`) && \
    CGO_ENABLED=0 go build -ldflags "-X main.version=${WEBPROC_LATEST_TAG}" -o /go/bin/webproc

### Stage 2: Final
FROM alpine:edge

LABEL maintainer.name="xczh" \
      maintainer.email="xczh.me@foxmail.com" \
      maintainer.description="Unpolluted DNS use dnsmasq and dnsproxy"

COPY --from=builder /go/bin/dnsproxy /go/bin/webproc /usr/local/bin/

RUN apk add --no-cache dnsmasq-dnssec

COPY root /

CMD ["/usr/local/bin/run-server"]