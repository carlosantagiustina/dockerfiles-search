FROM golang:latest as builder

RUN go get github.com/asobti/kube-monkey
WORKDIR $GOPATH/src/github.com/asobti/kube-monkey
RUN make build

FROM alpine
LABEL maintainer="asobti <ayushsobti@gmail.com>"

RUN	apk update && apk upgrade && apk add --no-cache bash tzdata

COPY --from=builder /go/src/github.com/asobti/kube-monkey/kube-monkey /kube-monkey
