FROM golang:1.13-alpine AS go-builder

# Replicate the APK repository override.
# If it is no longer necessary to avoid the CDN mirros we should consider dropping this as it is brittle.
RUN sed -e 's/dl-cdn[.]alpinelinux.org/mirrors.aliyun.com/g' -i~ /etc/apk/repositories
# Install our build time packages.
RUN apk add --no-cache build-base git openssh-client upx
# go proxy
RUN go env -w GOPROXY=https://goproxy.io

WORKDIR $GOPATH/src/github.com/kaisawind/template.go

COPY go.mod .
RUN go mod download

COPY . .

RUN make build-apiserver
RUN upx bin/apiserver -o bin/main
RUN chmod +x bin/main

FROM alpine
LABEL MAINTAINER="kaisawind <wind.kaisa@gmail.com>"

COPY --from=go-builder /go/src/github.com/kaisawind/template.go/bin/main /main
COPY --from=go-builder /go/src/github.com/kaisawind/template.go/apiserver/data/config-prod.yaml /template/config-prod.yaml

USER 6342627

CMD ["/main","--host=0.0.0.0","--port=8080"]
