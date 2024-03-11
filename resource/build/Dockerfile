FROM golang:1.21-bookworm AS builder
COPY . /go/src
WORKDIR /go/src
ENV GO111MODULE on
ENV GOPATH /go
ENV GOPROXY https://goproxy.io,direct
ENV GOPRIVATE *.zx-tech.net
ENV GOSUMDB off
ENV CGO_ENABLED 0
ENV GOINSECURE git.algor.tech
RUN go mod tidy
RUN go build -o app -v -ldflags "-s -w -X main.version='`git rev-parse --short HEAD`@`date '+%Y-%m-%d_%H:%M:%S_%Z_%z'`'" .

FROM debian:buster
RUN apt-get -y update && DEBIAN_FRONTEND="noninteractive" apt -y install tzdata
RUN apt-get -y install --no-install-recommends ca-certificates curl
LABEL maintainer="ohko <ohko@qq.com>"
COPY --from=builder /go/src/app /
WORKDIR /
ENV TZ Asia/Shanghai
EXPOSE 18080
ENTRYPOINT /app