# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0


FROM golang:1.19.2-alpine AS builder
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories
RUN apk add build-base protobuf-dev protoc
WORKDIR /usr/src/app/

# Restore dependencies
COPY ./src/accountingservice/ ./
COPY ./pb/ ./proto/
RUN go env -w GOPROXY=https://goproxy.cn,direct
RUN go mod download
RUN go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
RUN go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2

# Build executable
RUN protoc -I ./proto/ ./proto/demo.proto --go_out=./ --go-grpc_out=./
RUN go build -o /go/bin/accountingservice/ ./

# -----------------------------------------------------------------------------
# build image
FROM alpine:latest
WORKDIR /usr/src/app/
COPY --from=builder /go/bin/accountingservice/ ./
ENTRYPOINT [ "./accountingservice" ]
