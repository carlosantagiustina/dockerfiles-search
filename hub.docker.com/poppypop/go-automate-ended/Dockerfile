FROM golang:1.9-alpine as builder

RUN set -xe \
	&& apk update --no-cache && apk upgrade --no-cache \
	&& apk add --update --no-cache git \
	&& rm -rf /var/cache/apk/*

WORKDIR /go/src/github.com/PoppyPop/docker/webui-aria2/go-automate-ended
COPY ./src/ .

RUN go get -d -v ./...   
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o go-automate-ended .


FROM alpine:latest  
RUN apk add --no-cache --update ca-certificates tzdata unrar

VOLUME /datas

WORKDIR /app/
COPY --from=builder /go/src/github.com/PoppyPop/docker/webui-aria2/go-automate-ended/go-automate-ended .

ADD entrypoint.sh /entrypoint.sh
ENTRYPOINT [ "/entrypoint.sh" ]

CMD ["/app/go-automate-ended"] 
