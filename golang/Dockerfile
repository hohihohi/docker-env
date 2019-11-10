FROM golang:1.13-alpine

ENV PROJECT_PATH=github.com/hohihohi/docker-env/golang
ENV GOLANGCILINT_VERSION=1.21.0
ENV FIXUID_VERSION=0.4
# GO111MODULE=on is necessary, if golang version is 1.11 or 1.12
ENV GO111MODULE=on

WORKDIR /go/src/${PROJECT_PATH}

# Install some utility libraries
RUN set -ex && \
  apk update && \
  apk add --no-cache curl tar git gcc libc-dev make bash

RUN addgroup --gid 1000 docker && \
    adduser --uid 1000 --ingroup docker --home /home/docker --shell /bin/sh --disabled-password --gecos "" docker

# Fix uid
RUN USER=docker && \
    GROUP=docker && \
    curl -SsL https://github.com/boxboat/fixuid/releases/download/v${FIXUID_VERSION}/fixuid-${FIXUID_VERSION}-linux-amd64.tar.gz | tar -C /usr/local/bin -xzf - && \
    chown root:root /usr/local/bin/fixuid && \
    chmod 4755 /usr/local/bin/fixuid && \
    mkdir -p /etc/fixuid && \
    printf "user: $USER\ngroup: $GROUP\n" > /etc/fixuid/config.yml

USER docker:docker

# Install latest packages for development (golangci-lint)
RUN go get -u github.com/golangci/golangci-lint && \
  go get -u github.com/go-delve/delve/cmd/dlv && \
  go build -o /go/bin/dlv github.com/go-delve/delve/cmd/dlv

ENTRYPOINT ["fixuid", "-q"]
